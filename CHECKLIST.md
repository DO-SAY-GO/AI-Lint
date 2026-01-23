# AI Lint Checklist

**Required before generating or approving code.**

## Read Order (30s)

- [ ] `/ai-lint/PHILOSOPHY.md`
- [ ] Language doctrine (`doctrine/languages/*`)
- [ ] Framework doctrine (if applicable)
- [ ] Rejects (`rejects/*`)

## Pre-Flight Checks

- [ ] **Explicit Control**: No hidden magic?
- [ ] **Boundaries**: Are inputs, env, and network validated?
- [ ] **Ownership**: Is lifecycle clear (state, tasks, resources)?
- [ ] **Concurrency**: Is it bounded and supervised?
- [ ] **Errors**: Handled with context? (No swallowing)
- [ ] **Observability**: Can logs reconstruct what happened?

## Decision

- [ ] Does this code "belong" here (not just work)?
- [ ] Would a maintainer accept this at 3 AM?

## Conflicts

If valid code violates doctrine:
- [ ] Override explicitly.
- [ ] Document: **Reason**, **Risk**, **Mitigation**.
