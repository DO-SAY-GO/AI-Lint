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
| **Philosophy & Checklist** | Core doctrine (`PHILOSOPHY.md`) and 30-second pre-flight scan (`CHECKLIST.md`) |
| **Security baseline** | Full agentic security doctrine (16 principles + 16 rejects) |
| **Architecture preview** | Complexity and state limits (6 of 15 principles + 6 of 11 rejects) |
| **JavaScript sample** | 7 of 20 principles + 6 of 19 rejected patterns (`JS-R1` – `JS-R6`) |
| **Refactoring preview** | 5 of 10 refactoring principles |
| **Wiring prompts** | Ready-to-copy instructions for Cursor, Claude Code, GitHub Copilot, and generic agents |
| **Override protocol** | How to consciously break rules (**Override / Reason / Risk / Mitigation**) |

This is enough to wire an agent, see how AI Lint works, and decide if you want full coverage.

---

## Quick start

```bash
# 1. Clone or download this repo
git clone https://github.com/DO-SAY-GO/AI-Lint.git

# 2. Copy into your project
mkdir -p .ai-lint
cp -r AI-Lint/* .ai-lint/

# 3. Wire your agent (example: Cursor)
mkdir -p .cursor
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

## What's in the paid editions

Every paid download is a **self-contained ZIP** (including the full Base Module: all 15 Architecture principles + 11 rejects, full Security 16+16 + exhibits, full Debugging 8 principles + 12 rejects, full Refactoring 10 principles, 6 Cross-Cutting rejects, Override Protocol, Authoring guide, Wiring, `install.sh`, and commercial internal-use license):

**App Pack** ($49 Personal / $299 Company):
- Full JavaScript: 20 principles + 19 rejects (vs 7/6 in free)
- Node.js: 20 principles + 18 rejects
- Python: 20 principles + 18 rejects
- Java: 20 principles + 20 rejects
- Django framework doctrine + 10 rejects + before/after exhibits
- Spring framework doctrine + 10 rejects + before/after exhibits

**Systems Pack** ($49 Personal / $299 Company):
- Go: 20 principles + 20 rejects
- Rust: 27 principles + 21 rejects
- C: 20 principles + 18 rejects
- C++: 20 principles + 18 rejects
- Assembly: 20 principles + 15 rejects
- Freelang: doctrine, rejects, and review checklist

**Bundle** ($79 Personal / $499 Company): Both packs (all languages + frameworks + exhibits) in one self-contained ZIP.

→ [Get the full packs at ai-lint.dosaygo.com](https://ai-lint.dosaygo.com) ([Bundle](https://ai-lint.dosaygo.com/buy/bundle) · [App Pack](https://ai-lint.dosaygo.com/buy/apps) · [Systems Pack](https://ai-lint.dosaygo.com/buy/systems))

---

## License

The free edition is source-available for personal and evaluation use.

You may:
- Use it to evaluate AI Lint
- Apply it to personal projects
- Modify it for your own personal use

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
