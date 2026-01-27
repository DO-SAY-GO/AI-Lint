# Architecture Doctrine (Preview)

Code that works is not enough. Code must be **ownable**—understandable by one person in one sitting.

## 1. State Count Limit
**Reject**: More than 15-20 mutable state variables in a single file/module.
**Why**: Each variable is a dimension. 20 variables = 2²⁰ possible states. Undebuggable.
**Do**: If you need more, you have multiple modules pretending to be one. Split by concern.

## 2. One Problem, One Solution
**Reject**: Multiple approaches to the same problem coexisting (e.g., three "unblock" systems).
**Why**: Each approach has edge cases. Multiple approaches multiply edge cases.
**Do**: Pick one. Delete the others. If the new one fails, fix it—don't add a third.

## 3. Core Path Clarity
**Reject**: Business logic buried under defensive armor.
**Why**: The 80% case becomes invisible. New developers see only edge cases.
**Do**: Core path should be readable top-to-bottom in <100 lines. Edge handling is modules with on/off switches.

## 4. File Size Discipline
**Reject**: Files over 500 lines (hard limit: 1000).
**Why**: Large files are symptoms of mixed concerns. "I'll refactor later" never happens.
**Do**: When a file grows, extract. Don't wait until it's 4000 lines.

## 5. Explicit State Ownership
**Reject**: State scattered across file-level variables, closures, and object properties.
**Why**: "Where is this set?" becomes unanswerable.
**Do**: Single state object per concern. All mutations through it. Log on change if debugging.

## 6. Function Density
**Reject**: Functions over 50 lines (soft limit), 100 lines (hard limit).
**Why**: Long functions hide multiple responsibilities. "I need to read 100 lines to understand this."
**Do**: Extract. Name the extracted function for what it does, not when it's called.

## 7. Monotonic Observation
**Reject**: Conditioning capture/processing of external output on local input state.
**Why**: You silently drop or delay real events. Bugs become non-reproducible and “fixed” by timing.
**Do**: Always observe and record external output when it arrives. Gate **actions** (injections, commits) on local input state, not observation.

---

*This is a preview. The full Architecture Doctrine includes additional rules on dependency direction, timer accounting, synchronous initialization, feature flags, and complexity metrics tables.*

*Get the complete doctrine at https://ai-lint.dosaygo.com*
