# JavaScript / TypeScript Rejects

These are **not** lint rules. They are architectural constraints that keep JS/TS code **owned, testable, and debuggable**.

---

## JS-R1 — Layer Leaks (Framework Objects in Core)
**Reject**: Passing `Request/Response`, DOM nodes, framework models, or runtime globals into domain/services.
**Why**: Pollutes the core with I/O details; makes tests fake; spreads coupling.
**Do**: Translate at the boundary into plain domain types (DTOs/commands) and return plain results.

---

## JS-R2 — Side-Effect Imports
**Reject**: Modules that do work at import time (register handlers, start timers, read env, connect to DB).
**Why**: Ordering bugs, circular deps, “it depends on import order”, flaky tests.
**Do**: Export `init(...)` / `register(...)` and call it explicitly from the composition root.

---

## JS-R3 — Hidden Module Graph (Barrels That Obscure Dependencies)
**Reject**: “Re-export everything” barrel files that create invisible coupling (especially across layers).
**Why**: Makes dependency direction unclear; increases cycle risk; breaks tree-shaking/refactors.
**Do**: Use explicit public entrypoints per module, and import from them intentionally.

---

## JS-R4 — Circular Dependencies
**Reject**: Cycles between modules/packages (often via barrels or “shared” utils).
**Why**: Partial initialization, undefined exports, spooky runtime behavior.
**Do**: Break the cycle: move shared types down, depend on interfaces, or split modules by direction.

---

## JS-R5 — Deep Relative Imports (Path Spaghetti)
**Reject**: `../../../../` imports that cross boundaries.
**Why**: The folder layout becomes the architecture; refactors become landmines.
**Do**: Enforce module boundaries + stable entrypoints (monorepo packages or TS path aliases) and forbid cross-layer imports.

---

## JS-R6 — Unvalidated Boundaries
**Reject**: Trusting `req.body`, `process.env`, decoded JSON, or `as SomeType` without runtime validation.
**Why**: Types lie at runtime; invalid state enters core and explodes later.
**Do**: Validate at boundaries (schema validator) and convert to domain types immediately.

---

## JS-R7 — “Stringly” Errors
**Reject**: `throw "..."`, rejecting with strings/objects, or losing the causal chain.
**Why**: No stack discipline; no stable error taxonomy; impossible alerting/handling.
**Do**: Throw `Error` (or typed error classes) with `cause`, codes, and safe context.

---

## JS-R8 — Async Without Lifecycle (Fire-and-Forget)
**Reject**: Starting async work without tracking completion, cancellation, or error routing.
**Why**: Leaks, silent failures, and background work you can’t observe.
**Do**: Supervise tasks (queues/workers) and surface outcomes via logs/metrics + retries only when safe.

---

## JS-R9 — Missing Timeouts / Cancellation
**Reject**: External calls (HTTP/DB/queue) without timeouts, and long operations without `AbortSignal`.
**Why**: Cascading failures and wedged workers.
**Do**: Apply explicit timeouts and thread cancellation through call chains.

---

## JS-R10 — Unbounded Concurrency
**Reject**: `Promise.all` over unbounded inputs or fan-out without limits.
**Why**: Self-inflicted DoS (CPU, memory, sockets, DB pool).
**Do**: Use bounded pools/queues with backpressure and explicit concurrency limits.

---

## JS-R11 — Shared Mutable Singleton State
**Reject**: Module-level mutable state as a default (caches, registries, counters) without ownership + reset strategy.
**Why**: Test pollution, race conditions, non-determinism under load.
**Do**: Make state an injected dependency with clear lifecycle; isolate caches behind explicit interfaces.

---

## JS-R12 — Implicit Configuration Access
**Reject**: Reading env/config deep inside modules.
**Why**: Hidden inputs; behavior varies by deployment; impossible local reasoning.
**Do**: Load + validate config once at startup; pass a typed config object downward.

---

## JS-R13 — Non-Deterministic Core Logic
**Reject**: `Date.now()`, `Math.random()`, global clocks/UUIDs inside domain logic.
**Why**: Tests become flaky; replay/debug becomes hard.
**Do**: Inject `Clock`, `IdGenerator`, and randomness sources at the edge.

---

## JS-R14 — Tests That Lie (Mock Everything)
**Reject**: Unit tests that only assert mocks/snapshots and never exercise wiring, boundaries, and contracts.
**Why**: “Green tests, broken system.”
**Do**: Prefer a pyramid: unit tests for pure core + integration/contract tests for boundaries + a few end-to-end checks.

---

## JS-R15 — Logs Without Causality
**Reject**: `console.log` strings without trace/request IDs, intent, and outcome.
**Why**: Noise instead of observability.
**Do**: Use structured logs + correlation IDs and log events (intent → result) with stable fields.

---

## JS-R16 — Streaming Without Backpressure
**Reject**: Processing data streams (file uploads, WebSocket messages, database cursors) without explicit backpressure handling.
**Why**: Fast producer + slow consumer = memory exhaustion. This manifests as "works in dev, OOM in prod."
**Do**: Use `pause()`/`resume()` for Node streams, explicit buffer limits, or libraries with built-in backpressure (RxJS `bufferTime`, async iterators with bounded queues).

---

## JS-R17 — Async State Races (Check-Then-Act)

**Reject:**
```javascript
async function withdraw(accountId, amount) {
  const balance = await getBalance(accountId);  // Check
  if (balance >= amount) {
    await setBalance(accountId, balance - amount);  // Act
  }
}
// Two concurrent withdrawals can both pass the check before either acts
```

**Why:** JavaScript is single-threaded but async is not atomic. Between `await` calls, other async operations can interleave. This is the async equivalent of a data race—it passes all tests but fails under concurrent requests.

**Do:**
```javascript
async function withdraw(accountId, amount) {
  // Use database-level atomicity
  const result = await db.query(
    'UPDATE accounts SET balance = balance - $1 WHERE id = $2 AND balance >= $1 RETURNING balance',
    [amount, accountId]
  );
  if (result.rowCount === 0) {
    throw new InsufficientFundsError();
  }
}
// Or use a distributed lock if cross-system
```

---

## JS-R18 — Shared State in Concurrent Request Handlers

**Reject:**
```javascript
let requestCount = 0;
let lastUser = null;

app.post('/action', async (req, res) => {
  requestCount++;
  lastUser = req.user;  // RACE: another request overwrites this
  
  await doExpensiveWork();
  
  // lastUser is now wrong—it's from a different request
  await auditLog(`${lastUser} performed action`);
});
```

**Why:** Each request handler runs in the same event loop. Shared mutable state is visible to all concurrent requests. After any `await`, another request may have modified it.

**Do:**
```javascript
app.post('/action', async (req, res) => {
  const user = req.user;  // Capture in local scope
  
  await doExpensiveWork();
  
  await auditLog(`${user} performed action`);  // Uses captured value
});

// For truly shared state (counters, caches), use atomic operations or proper isolation
```

---

## JS-R19 — Web Worker Communication Without Error Handling

**Reject:**
```javascript
const worker = new Worker('worker.js');
worker.postMessage({ data: largeArray });
// Fire and forget; no error handling, no timeout
```

**Why:** Workers can crash, throw, or hang. Without `onerror` and `onmessage` handlers, failures are silent. Without timeouts, a stuck worker blocks the feature indefinitely.

**Do:**
```javascript
const worker = new Worker('worker.js');

const result = await new Promise((resolve, reject) => {
  const timeout = setTimeout(() => {
    worker.terminate();
    reject(new Error('Worker timeout'));
  }, 30000);
  
  worker.onmessage = (e) => {
    clearTimeout(timeout);
    resolve(e.data);
  };
  
  worker.onerror = (e) => {
    clearTimeout(timeout);
    reject(new Error(`Worker error: ${e.message}`));
  };
  
  worker.postMessage({ data: largeArray });
});
```
