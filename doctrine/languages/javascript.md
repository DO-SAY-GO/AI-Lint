# JavaScript Doctrine

JavaScript is permissive. That permissiveness creates entropy unless boundaries are explicit.

## 1. Explicit Async
**Reject**: Implicit async via callbacks/emitters when `async/await` works.
**Why**: Obscures execution order and error propagation.
**Do**: Use `async/await`. Make async boundaries explicit.

## 2. No Swallowed Errors
**Reject**: Empty `catch`, silent defaults, logging without context.
**Why**: Destroys causality. Makes debugging nonlinear.
**Do**: Handle, rethrow with context, or convert to domain error.

## 3. Stable Data Shapes
**Reject**: Ad-hoc shapes, optional fields without validation, duck typing.
**Why**: Drift becomes invisible coupling.
**Do**: Define stable schemas (TS/JSDoc). Validate at boundaries.

## 4. No Implicit Global State
**Reject**: Mutable singletons, hidden caches.
**Why**: Erases ownership. Breaks tests.
**Do**: Dependency injection or explicit state owners.

## 5. No Event-Loop Hacks
**Reject**: `setTimeout(0)`, `queueMicrotask` ordering tricks.
**Why**: Brittle under load/refactor.
**Do**: Explicit queues or state machines.

## 6. Bounded Concurrency
**Reject**: Unbounded `Promise.all`.
**Why**: Resource exhaustion.
**Do**: Explicit limits, pooling, backpressure.

## 7. No `any` Defaults
**Reject**: Broad `any` usage.
**Why**: Erases semantics. Pushes bugs to runtime.
**Do**: Narrow `unknown` at edges immediately.

## 8. Meaningful Abstraction
**Reject**: Wrappers that only rename APIs.
**Why**: Increases cognitive load.
**Do**: Abstract only to reduce complexity or enforce invariants.

## 9. Pure Core
**Reject**: Mixing I/O and mutation in core logic.
**Why**: Kills testability.
**Do**: Pure core + explicit I/O edges.

## 10. Causal Logs
**Reject**: Logs without identifiers/intent.
**Why**: Noise.
**Do**: Intent + IDs + Outcome.

## 11. No Boolean Flags
**Reject**: `doThing(true, false)`.
**Why**: Ambiguous. Combinatorial paths.
**Do**: Options objects or distinct functions.

## 12. Typed Domain Values
**Reject**: "Stringly typed" logic.
**Why**: Invalid states.
**Do**: Enums, union types, tagged types.

## 13. Explicit Config
**Reject**: `process.env` deep in modules.
**Why**: Environment coupling.
**Do**: Load once, pass explicit config object.

## 14. Consistent Sync/Async
**Reject**: Functions that mix sync return/promises.
**Why**: Zalgo. Timing bugs.
**Do**: Consistently sync or async.

## 15. No Hidden Mutation
**Reject**: Mutating arguments without docs.
**Why**: Action-at-a-distance.
**Do**: Immutability or explicit ownership transfer.

## 16. Actionable Errors
**Reject**: "Invalid input".
**Why**: Undebuggable.
**Do**: Context + Rule + Path.

## 17. No Null/Undefined Ambiguity
**Reject**: Optional values flowing deep.
**Why**: Invariants become unclear.
**Do**: Normalize at boundaries.

## 18. Complete Validation
**Reject**: Partial/ad-hoc validation.
**Why**: Security bugs. False confidence.
**Do**: Complete schema validation at boundaries.

## 19. Readable Logic
**Reject**: Clever one-liners.
**Why**: Hard to debug.
**Do**: Named intermediate steps.

## 20. Managed Async Lifecycles
**Reject**: Fire-and-forget.
**Why**: Leaks. Silent failures.
**Do**: Tracked tasks with error paths.

---

## Known Tensions

These principles sometimes conflict. When they do, surface the tradeoff explicitly.

| Tension | Example | Resolution Guidance |
|---------|---------|---------------------|
| **Explicit Config** vs **Framework Conventions** | Next.js expects `NEXT_PUBLIC_*` env vars; Vite expects `VITE_*` | Framework convention wins at framework boundaries. Document the coupling. |
| **Pure Core** vs **Framework Patterns** | React hooks mix state and effects by design | Isolate impure hooks at component edges; keep business logic pure. |
| **Stable Data Shapes** vs **Rapid Prototyping** | Early-stage code needs flexibility | Use `Partial<T>` with explicit `// PROTOTYPE: shape unstable` comments. Remove before production. |
| **No `any` Defaults** vs **Third-Party Types** | Library has incomplete types | Narrow at the boundary with explicit `as unknown as T` and a comment explaining the gap. |
| **Bounded Concurrency** vs **Throughput** | Batch job needs maximum parallelism | Override allowed with documented resource limits and monitoring. |
