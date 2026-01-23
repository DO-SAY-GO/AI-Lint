# Wiring — Connect your AI agent to AI Lint

This folder contains ready-to-paste instructions for AI coding agents.

## Pick the file that matches your setup

| Agent | File | Where to paste it |
|-------|------|-------------------|
| Cursor | `AGENTS.md` | Copy to `.cursor/AGENTS.md` or paste into Cursor settings → Rules |
| Claude Projects | `CLAUDE.md` | Paste into project instructions |
| GitHub Copilot | `COPILOT.md` | Copy to `.github/copilot-instructions.md` |
| Windsurf / Other | `AI_INSTRUCTIONS.md` | Copy to your agent's instruction file |

## What the wiring does

Once wired, your agent will:

1. **Read doctrine first** — before generating code, it consults `.ai-lint/`
2. **Check rejects** — refuses to use patterns marked as rejected
3. **Ask about overrides** — if a reject seems necessary, it surfaces the tradeoff instead of silently violating doctrine
4. **Cite its reasoning** — when proposing architecture, it references which doctrine entries motivated the decision

## The 30-second setup

```bash
# 1. Create the doctrine folder in your repo
mkdir .ai-lint

# 2. Copy AI Lint files into it (adjust based on your pack)
cp -r ai-lint-base/* .ai-lint/
cp -r ai-lint-pack-apps/* .ai-lint/     # if you have Apps pack
cp -r ai-lint-pack-systems/* .ai-lint/  # if you have Systems pack

# 3. Wire your agent (example: Cursor)
cp .ai-lint/wiring/AGENTS.md .cursor/AGENTS.md
```

That's it. Your agent now consults AI Lint before writing code.

## Testing the wiring

After setup, ask your agent to do something that violates a reject. For example:

> "Write a JavaScript module that uses a global variable to track state"

A properly wired agent should:
1. Flag that this violates JS-R1 (implicit global state)
2. Explain why the reject exists
3. Offer an alternative, or ask if you want to override

If it just writes the code without comment, the wiring isn't working.