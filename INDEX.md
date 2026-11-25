# AI Lint — Index (Free Edition)

This is the doctrine layer for this repository.

## Quick start for agents

1. Read `PHILOSOPHY.md` — the governing principles
2. Read `doctrine/architecture.md` — complexity management (preview)
3. Read `doctrine/languages/javascript.md` — what belongs (sample)
4. Read `rejects/architecture.md` — architectural anti-patterns (preview)
5. Read `rejects/languages/javascript.md` — what to refuse (sample)
6. Follow the override protocol if you need to break a rule

---

## File map

```
.ai-lint/
├── PHILOSOPHY.md          # Core doctrine (read first)
├── CHECKLIST.md           # Quick pre-code scan
├── INDEX.md               # This file
│
├── doctrine/
│   ├── architecture.md    # Complexity management (6 of 10 - preview)
│   ├── languages/
│   │   └── javascript.md  # JS doctrine (7 of 20 - sample)
│   └── security.md        # Security doctrine (full)
│
├── rejects/
│   ├── architecture.md    # Architectural anti-patterns (6 of 10 - preview)
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
2. **Architecture doctrine** (complexity, state management)
3. **Framework doctrine** (not in free edition)
4. **Language doctrine**
5. **Core philosophy**
6. **Generic best practices** (weakest)

---

## Want full coverage?

This free edition includes JavaScript and architecture previews.

Full packs (self-contained ZIPs including full Architecture 15+11, Debugging 8+12, Refactoring 10, Cross-Cutting 6, and Security exhibits):
- **App Pack** ($49 Personal / $299 Company): JavaScript (20+19), Node.js (20+18), Python (20+18), Java (20+20), Django (doctrine + 10 rejects + exhibits), Spring (doctrine + 10 rejects + exhibits)
- **Systems Pack** ($49 Personal / $299 Company): Go (20+20), Rust (27+21), C (20+18), C++ (20+18), Assembly (20+15)
- **Bundle** ($79 Personal / $499 Company): App Pack + Systems Pack in one self-contained ZIP

→ [ai-lint.dosaygo.com](https://ai-lint.dosaygo.com)
