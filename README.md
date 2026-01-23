# AI Lint — Free Edition

**Teach your AI agents the difference between code that "works" and code that "belongs."**

AI Lint is doctrine for AI coding agents. It externalizes senior engineering judgment—so agents build code that fits the language, the framework, and your codebase.

AI models are great at syntax but terrible at taste. They invent unnecessary abstractions, hide complexity, and create technical debt that looks "correct" but rots quickly.

AI Lint fixes this by injecting explicit architectural constraints into the agent's context.

The goal of AI Lint is to dramatically improve codebase quality, while reducing task time-to-completion for agentic coding. It accomplishes this by reducing the cycle of generating, reviewing, rejecting, and prompting again. AI Lint constrains the search space so agents are more likely to get it right the first time.

---

## What's in the free edition

| Component | Description |
|-----------|-------------|
| **Philosophy** | Core doctrine: clarity over cleverness, visible complexity, debuggable causality |
| **Security baseline** | Agentic security doctrine (16 principles) |
| **JavaScript doctrine** | 7 principles (sample from 20) |
| **JavaScript rejects** | 6 rejected patterns (sample from 19) |
| **Wiring prompts** | Ready-to-paste instructions for Cursor, Claude, Copilot |
| **Override protocol** | How to consciously break rules |

This is enough to wire an agent, see how AI Lint works, and decide if you want full coverage.

---

## Quick start

```bash
# 1. Clone or download this repo
git clone https://github.com/yourorg/ai-lint.git

# 2. Copy into your project
mkdir .ai-lint
cp -r ai-lint/* .ai-lint/

# 3. Wire your agent (example: Cursor)
cp .ai-lint/wiring/AGENTS.md .cursor/AGENTS.md
```

That's it. Your agent now consults AI Lint before writing JavaScript.

---

## Verify it works

Ask your agent:

> "Write a JavaScript module that uses a global variable to track user sessions"

A properly wired agent should:
1. Flag that this violates **JS-R1** (implicit global state)
2. Explain why: it hides causality and breaks test reset
3. Offer an alternative, or ask if you want to override

---

## What's NOT in the free edition

The free edition covers JavaScript. Paid packs cover:

**App Pack** ($49):
- Full JavaScript: 20 principles + 19 rejects (vs 7/6 in free)
- Node.js doctrine + 18 rejects
- Python doctrine + 18 rejects
- Java doctrine + 20 rejects
- Django framework doctrine + 10 rejects
- Spring framework doctrine + 10 rejects

**Systems Pack** ($49):
- Go doctrine + 20 rejects
- Rust doctrine + 18 rejects
- C doctrine + 18 rejects
- C++ doctrine + 18 rejects
- Assembly doctrine + 15 rejects

**Bundle** ($79): Both packs + all future packs

→ [Get the full packs](https://ailint.dev)

---

## License

The free edition is source-available for evaluation and internal use.

You may:
- Use it to evaluate AI Lint
- Apply it to personal/internal projects
- Modify it for your own use

You may NOT:
- Redistribute or resell
- Use to train AI/ML models
- Use in commercial consulting/education offerings

For commercial use with full language coverage, purchase a pack.

---

## Why AI Lint exists

AI models produce code that compiles. Then you review it and feel the familiar dread: it's not wrong enough to reject quickly—just wrong enough to quietly poison the codebase.

The problem isn't syntax. It's judgment.

AI Lint makes that judgment explicit. It tells your agent what patterns belong, what should be rejected, and when to ask a human instead of guessing.

---

*"Works" isn't good enough. The goal is code that belongs.*
