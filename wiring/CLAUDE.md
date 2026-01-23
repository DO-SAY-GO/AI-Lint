# Claude Instructions (AI Lint)

This repo uses **AI Lint** as binding doctrine.

Before producing code:
- Read `.ai-lint/INDEX.md`
- Read `.ai-lint/PHILOSOPHY.md`
- Read `.ai-lint/doctrine/languages/<language>.md` (and `.ai-lint/doctrine/frameworks/<framework>.md` if applicable)
- Read `.ai-lint/rejects/languages/<language>.md` and `.ai-lint/rejects/security.md`

Priority:
1) AI Lint rejects
2) AI Lint language/framework doctrine
3) General best practices

Do not use patterns listed as rejected unless an override is explicitly requested.
If an override may be required, consult `.ai-lint/overrides/OVERRIDE-PROTOCOL.md` and propose it using:
Override / Reason / Risk / Mitigation.

Aim for code that **belongs** (idiomatic, maintainable, debuggable), not code that merely works.

