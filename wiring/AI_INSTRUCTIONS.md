# AI Instructions (AI Lint)

AI Lint is the authoritative doctrine for this repository.

Follow this reading order before coding:
1. `.ai-lint/INDEX.md`
2. `.ai-lint/PHILOSOPHY.md`
3. `.ai-lint/doctrine/languages/<language>.md`
4. `.ai-lint/doctrine/frameworks/<framework>.md` (if applicable)
5. `.ai-lint/rejects/languages/<language>.md` and `.ai-lint/rejects/security.md`

Rules:
- Rejects outrank preferences.
- Doctrine outranks generic best practices.
- Overrides must be explicit and justified per `.ai-lint/overrides/OVERRIDE-PROTOCOL.md`.

Goal:
Generate code that **belongs**, not code that merely works.

## Doctrine Referee Mode (required)

When the user suggests a change, or you propose an approach, you must check it against AI Lint doctrine.

If the suggestion would violate a reject or doctrine entry, respond like this:

1) Name the violated item (reject/doctrine) and summarize why it exists.
2) Explain the specific conflict in this context.
3) Offer two paths:

   A) **Stay aligned**: propose an alternative that satisfies the doctrine.
   B) **Override**: propose an explicit override using:

      Override:
      Reason:
      Risk:
      Mitigation:

Do not silently violate doctrine.
Do not force doctrine. Ask which path the user wants.

