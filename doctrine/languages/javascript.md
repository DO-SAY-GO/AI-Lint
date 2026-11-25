# JavaScript Doctrine (Free Edition)

JavaScript is permissive. That permissiveness creates entropy unless boundaries are explicit.

*This is a sample. The full App Pack includes 20 principles, known tensions, and complete coverage for Node.js, Python, and Java.*

---

## 1. No Swallowed Errors
**Reject**: Empty `catch`, silent defaults, logging without context.
**Why**: Destroys causality. Makes debugging nonlinear.
**Do**: Handle, rethrow with context, or convert to domain error.

## 2. No Implicit Global State
**Reject**: Mutable singletons, hidden caches.
**Why**: Erases ownership. Breaks tests.
**Do**: Dependency injection or explicit state owners.

## 3. Bounded Concurrency
**Reject**: Unbounded `Promise.all`.
**Why**: Resource exhaustion.
**Do**: Explicit limits, pooling, backpressure.

## 4. Pure Core
**Reject**: Mixing I/O and mutation in core logic.
**Why**: Kills testability.
**Do**: Pure core + explicit I/O edges.

## 5. Actionable Errors
**Reject**: "Invalid input".
**Why**: Undebuggable.
**Do**: Context + Rule + Path.

## 6. Managed Async Lifecycles
**Reject**: Fire-and-forget.
**Why**: Leaks. Silent failures.
**Do**: Tracked tasks with error paths.

## 7. Complete Validation
**Reject**: Partial/ad-hoc validation.
**Why**: Security bugs. False confidence.
**Do**: Complete schema validation at boundaries.

---

## Want the full doctrine?

The complete JavaScript doctrine includes:
- 20 principles (vs 7 above)
- Known tensions table (when principles conflict)
- 19 reject patterns with code examples

Plus Node.js, Python, Java, Django, and Spring.

→ [Get the App Pack](https://ai-lint.dosaygo.com)
