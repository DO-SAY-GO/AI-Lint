# AI Lint — Index (Free Edition)

This is the doctrine layer for this repository.

## Quick start for agents

1. Read `PHILOSOPHY.md` — the governing principles
2. Read `doctrine/languages/javascript.md` — what belongs (sample)
3. Read `rejects/languages/javascript.md` — what to refuse (sample)
4. Follow the override protocol if you need to break a rule

---

## File map

```
.ai-lint/
├── PHILOSOPHY.md          # Core doctrine (read first)
├── CHECKLIST.md           # Quick pre-code scan
├── INDEX.md               # This file
│
├── doctrine/
│   ├── languages/
│   │   └── javascript.md  # JS doctrine (7 of 20 - sample)
│   └── security.md        # Security doctrine (full)
│
├── rejects/
│   ├── languages/
│   │   └── javascript.md  # JS rejects (6 of 19 - sample)
│   └── security.md        # Security rejects (full)
│
├── overrides/
│   └── OVERRIDE-PROTOCOL.md
│
└── wiring/
    ├── AGENTS.md          # For Cursor
    ├── CLAUDE.md          # For Claude
    ├── COPILOT.md         # For GitHub Copilot
    └── AI_INSTRUCTIONS.md # Generic
```

---

## Authority order

When rules conflict, apply this precedence:

1. **Rejects** (strongest)
2. **Framework doctrine** (not in free edition)
3. **Language doctrine**
4. **Core philosophy**
5. **Generic best practices** (weakest)

---

## Want full coverage?

This free edition includes JavaScript only (7 doctrine, 6 rejects).

Full packs include:
- **App Pack**: JS (20+19), Node.js, Python, Java, Django, Spring
- **Systems Pack**: Go, Rust, C, C++, Assembly

→ [ailint.dev](https://ailint.dev)
