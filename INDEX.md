# AI Lint — Index

## How to use this

1. Read `PHILOSOPHY.md` — the governing principles
2. Read `doctrine/languages/javascript.md` — what belongs in JS
3. Read `rejects/languages/javascript.md` — what to refuse
4. Wire your agent using a file from `wiring/`

---

## File map

```
.ai-lint/
├── README.md              # This file
├── PHILOSOPHY.md          # Core doctrine
├── CHECKLIST.md           # Quick pre-code scan
├── doctrine/
│   ├── security.md        # Security doctrine
│   └── languages/
│       └── javascript.md  # JS doctrine (free)
├── rejects/
│   ├── security.md        # Security rejects
│   └── languages/
│       └── javascript.md  # JS rejects (free)
├── overrides/
│   └── OVERRIDE-PROTOCOL.md
└── wiring/
    ├── AGENTS.md          # For Cursor
    ├── CLAUDE.md          # For Claude Projects
    ├── COPILOT.md         # For GitHub Copilot
    └── AI_INSTRUCTIONS.md # Generic
```

---

## Authority order

When rules conflict, apply this precedence:

1. **Rejects** (strongest)
2. **Framework doctrine** (if applicable)
3. **Language doctrine**
4. **Core philosophy**
5. **Generic best practices** (weakest)

If in doubt, surface the tradeoff. Don't guess silently.

---

## Want more languages?

This free edition includes JavaScript only.

Full coverage (Python, Java, Node.js, Go, Rust, C, C++, Django, Spring, and more) is available in paid packs.

→ [ailint.dev](https://ailint.dev)
