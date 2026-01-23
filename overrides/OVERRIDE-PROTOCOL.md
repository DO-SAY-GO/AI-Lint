# Override Protocol (AI Lint)

AI Lint is a default doctrine, not a religion.

Overrides are allowed.
But they must be explicit, justified, localized, and risk-aware.

This protocol exists to:
- preserve agency
- prevent dogma
- prevent silent drift
- keep tradeoffs conscious

---

## Priority order (authority)

When deciding what belongs, follow this precedence:

1) **Rejects** (`/ai-lint/rejects/*`)
2) **Framework doctrine** (`/ai-lint/doctrine/frameworks/*`) when applicable
3) **Language doctrine** (`/ai-lint/doctrine/languages/*`)
4) **Core philosophy** (`/ai-lint/PHILOSOPHY.md`)
5) Generic best practices / model defaults

If there is conflict, higher priority wins by default.

---

## When overrides are appropriate

Overrides may be justified when:
- performance constraints dominate and are measured
- external protocols/ABIs impose constraints
- legacy boundaries cannot be changed safely
- experimental code is isolated and disposable
- a well-understood local exception improves clarity

Overrides are NOT appropriate for:
- convenience
- aesthetics
- avoiding short-term effort
- “this is how I usually do it” without a concrete reason

---

## Override format (required)

When overriding doctrine, document the decision using this exact format:

Override: <rule name or reject ID being violated>
Reason: <why the override is necessary in this context>
Risk: <what hazard this introduces (correctness, security, maintainability, etc.)>
Mitigation: <how the risk is limited, monitored, tested, or contained>

Overrides must be placed as close as possible to the affected code
(comment, docblock, commit message, or local design note).

---

## Contradictions in taste (required procedure)

Sometimes doctrine preferences can conflict, or multiple doctrines may apply.

When you detect a conflict, you must:

1) **State the conflict explicitly**
   - “Rule A suggests X; Rule B suggests Y.”

2) **Identify the layer of each rule**
   - reject vs framework vs language vs philosophy

3) **Propose the default resolution** using priority order
   - “Default is X because reject/framework outranks language preference.”

4) **Offer two paths**
   A) Stay aligned: propose an alternative approach that satisfies doctrine  
   B) Override: propose a documented override (template above)

Do not silently pick a side when doctrine conflicts.
Surface the tradeoff and ask which path to take.

---

## Agent “doctrine referee” behavior (required)

When the user proposes an approach, or you propose one, you must check it against AI Lint.

If it violates a reject or doctrine entry:
- name the violated rule
- explain why it exists (briefly)
- offer: **stay aligned** vs **override**
- if override: require the override template

This is not bureaucracy.
This is how we keep the system debuggable and the tradeoffs explicit.

