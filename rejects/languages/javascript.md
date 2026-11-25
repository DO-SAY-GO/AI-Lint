# JavaScript Rejects (Free Edition)

These are **not** lint rules. They are architectural constraints that keep JS/TS code **owned, testable, and debuggable**.

*This is a sample. The full App Pack includes 19 rejects with detailed code examples.*

---

## JS-R1 — Shared Mutable Singleton State
**Reject**: Module-level mutable state as a default (caches, registries, counters) without ownership + reset strategy.
**Why**: Test pollution, race conditions, non-determinism under load.
**Do**: Make state an injected dependency with clear lifecycle; isolate caches behind explicit interfaces.

---

## JS-R2 — Unvalidated Boundaries
**Reject**: Trusting `req.body`, `process.env`, decoded JSON, or `as SomeType` without runtime validation.
**Why**: Types lie at runtime; invalid state enters core and explodes later.
**Do**: Validate at boundaries (schema validator) and convert to domain types immediately.

---

## JS-R3 — Async Without Lifecycle (Fire-and-Forget)
**Reject**: Starting async work without tracking completion, cancellation, or error routing.
**Why**: Leaks, silent failures, and background work you can't observe.
**Do**: Supervise tasks (queues/workers) and surface outcomes via logs/metrics + retries only when safe.

---

## JS-R4 — Missing Timeouts / Cancellation
**Reject**: External calls (HTTP/DB/queue) without timeouts, and long operations without `AbortSignal`.
**Why**: Cascading failures and wedged workers.
**Do**: Apply explicit timeouts and thread cancellation through call chains.

---

## JS-R5 — "Stringly" Errors
**Reject**: `throw "..."`, rejecting with strings/objects, or losing the causal chain.
**Why**: No stack discipline; no stable error taxonomy; impossible alerting/handling.
**Do**: Throw `Error` (or typed error classes) with `cause`, codes, and safe context.

---

## JS-R6 — Logs Without Causality
**Reject**: `console.log` strings without trace/request IDs, intent, and outcome.
**Why**: Noise instead of observability.
**Do**: Use structured logs + correlation IDs and log events (intent → result) with stable fields.

---

## Want the full rejects?

The complete JavaScript rejects include:
- 19 patterns (vs 6 above)
- Detailed code examples (before/after)
- Async race conditions, backpressure, Web Workers, and more

Plus Node.js (18 rejects), Python, Java, Django, and Spring.

→ [Get the App Pack](https://ai-lint.dosaygo.com)
