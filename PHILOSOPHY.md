# Core Philosophy — AI Lint Doctrine

**Complexity must be earned.**

AI Lint exists to enforce the difference between code that "runs" and code that "belongs."

---

## 1. We do not hide complexity; we surface it.
If complexity cannot be removed, it must be made visible in the code’s shape, naming, and control flow.

## 2. No magic.
Avoid designs where behavior depends on invisible conventions, implicit runtime wiring, or “it just happens.”

## 3. Prefer clarity over cleverness.
Optimize for the next reader at 3am, not the author at 3pm.

## 4. Make causality debuggable.
You should be able to answer: “what happened, in what order, and why” using logs, traces, and a debugger.

## 5. Make risk explicit.
When violating a rule, document the tradeoff and the hazard you’re accepting.

---

## The Override Protocol
AI Lint is a doctrine, not a straitjacket. You may break any rule if:
1. You acknowledge the rule you are breaking.
2. You document *why* the deviation is necessary for this specific context.
3. You mitigate the specific risk the rule was designed to prevent.
