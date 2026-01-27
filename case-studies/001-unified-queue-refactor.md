# Case Study: Unified Queue Refactor (ai-chat.email)

**Date:** January 2026  
**Project:** ai-chat.email (PTY-based AI chat wrapper)  
**Duration:** ~2 hours  
**Outcome:** Successful migration from 5+ competing queue mechanisms to single unified queue

---

## Problem Statement

The codebase had accumulated multiple competing mechanisms for queuing content to be sent to the AI agent:

1. `state.pendingHumanCommit` - flag for deferred commits
2. `state.pendingReminderTexts[]` - array of reminder strings
3. `state.pendingInboundMessages[]` - array of email messages
4. `state.pendingTurnContent` - string accumulator
5. `core.queue[]` - generic queue in conversation-core
6. Various `buildPendingInjection()` functions that combined these inconsistently

This violated the architecture doctrine principle AR-R2 (Parallel Solutions): "When two subsystems solve the same problem (caching, rate limiting, queue management), merge them."

### Symptoms
- Emails sometimes not auto-committed
- Reminders firing at wrong times
- Race conditions between queue mechanisms
- Unclear which queue to use for new features

---

## AI-Lint Evaluation (Pre-Refactor)

Before beginning, we evaluated the planned changes against ai-lint doctrine:

| Principle | Status | Notes |
|-----------|--------|-------|
| AR-R2 Parallel Solutions | ❌ Violated | 5+ competing queue mechanisms |
| Single Source of Truth | ❌ Violated | Content scattered across multiple arrays |
| Turn Integrity | ⚠️ At Risk | Multiple paths to commit content |

This evaluation crystallized the problem and justified a unified approach.

---

## Solution Design

### Unified Queue Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    waitingTurns[]                            │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │ { type, text, timestamp, interrupt, predicate, meta }   │ │
│  └─────────────────────────────────────────────────────────┘ │
│                                                              │
│  Types: 'email' | 'user' | 'handoff.*'                      │
│  Predicates: evaluated at drain time (handoff.* only)       │
│  Interrupt: flag for urgent content requiring agent stop    │
└─────────────────────────────────────────────────────────────┘
```

### Key Design Decisions

1. **Everything flows through single queue** - No bypasses, even for urgent emails
2. **Predicates evaluated at drain time** - Not at enqueue time (state may change)
3. **User text first, system items chronological** - Consistent output ordering
4. **Dedupe by type+text for handoff items** - Emails never dedupe
5. **Try/catch around drain** - Preserve content on error

---

## Implementation

### Files Created
- `lib/waiting-turns.js` (375 lines) - New unified queue module

### Files Modified
- `lib/state.js` - Removed 4 pending variables, added `waitingTurns[]`
- `lib/conversation-core.js` - Removed ~200 lines of queue logic
- `lib/tui/input-handler.js` - Simplified commit flow
- `lib/tui/agent-lifecycle.js` - Use enqueueHandoff for reminders
- `lib/tui/injection.js` - Route through waitingTurns
- `lib/tui/threading.js` - All emails flow through queue
- `lib/tui/interaction-policy.js` - Removed buildPendingInjection
- `lib/tui/runtime.js` - Wire up waitingTurns
- `lib/tui/pty-runtime.js` - Add waitingTurns to context

### Migration Safety
- Created git tag `pre-waiting-turns-refactor` before starting
- Syntax checked after each file change
- Committed incrementally with descriptive messages

---

## Post-Migration Bug: Double-Send

After initial migration, testing revealed a critical bug:
- User types `/model`, presses Enter
- Text appears **twice** in terminal
- Agent never responds

### Root Cause Analysis

PTY logs revealed the issue:
```
[PTY-WRITE] reason=raw-key-batch preview="/" bytes=1
[PTY-WRITE] reason=raw-key-batch preview="m" bytes=1
...
[PTY-WRITE] reason=human-commit preview="/model\r" bytes=7  ← DUPLICATE!
```

Keystrokes were being sent to PTY in real-time via `raw-key-batch`, then the same text was sent again on Enter via `human-commit`.

### Fix

Added `userOnlyTyped` flag to drain() return value:
```javascript
return {
  message,
  hadContent: message.trim().length > 0,
  hadInterrupt: hasInterrupt,
  userOnlyTyped: userItems.length > 0 && systemItems.length === 0
};
```

In commitHumanInput:
```javascript
if (userOnlyTyped) {
  // Text already in PTY, just send Enter
  writeToPty('\r', 'human-commit-enter');
} else {
  // Bundled content - clear line and send full bundle
  writeToPty('\x15', 'clear-line-before-bundle');
  writeToPty(message + '\r', 'human-commit-bundle');
}
```

---

## Key Learnings

### 1. Doctrine Prevents Wrong Designs, Testing Catches Wrong Implementations

> "Forcing ai-lint evaluation made me think structurally before coding"

AI-lint doctrine correctly identified the architectural problem (AR-R2 violation) and justified the unified approach. But it couldn't catch the double-send bug - that required actual testing with ground-truth observation.

### 2. "Don't Make Changes, Propose Them"

When the refactor revealed cascading issues, the human guidance to "propose changes, don't make them" prevented premature patches that would have added complexity without fixing root causes.

### 3. Ground-Truth Testing Trumps Log Analysis

The logs showed `[COMMIT] Turn finalized after PTY write` - superficially indicating success. But the human's observation "agent never did anything" forced deeper investigation that revealed the actual bug.

### 4. Migration Checkpoints Are Essential

The pre-migration git tag allowed confident experimentation knowing we could revert cleanly.

---

## AI-Lint Compliance (Post-Refactor)

| Category | Score | Notes |
|----------|-------|-------|
| Architecture | 7/7 ✅ | AR-R2 fixed, single source of truth |
| Security | 5/5 ✅ | No new attack surface |
| JavaScript | 6/7 ✅ | One minor (error return values) |

---

## Commits

1. `9cf1526` - Refactor: unified waitingTurns queue replaces multiple pending states
2. `649385b` - Add interrupt drain sequence, remove deprecated buildPendingInjection
3. `4c9f1c6` - Fix double-send bug: user text already in PTY, only send Enter

---

## Conclusion

The unified queue refactor successfully eliminated architectural debt while maintaining functionality. The combination of:
- **Doctrine evaluation** (structural thinking before coding)
- **Human testing feedback** (ground-truth bug detection)
- **Incremental migration** (checkpoints and verification)

...produced a cleaner, more maintainable system. Neither AI-lint doctrine nor testing alone would have achieved this result - the combination was essential.
