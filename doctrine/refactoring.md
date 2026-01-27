# Refactoring Doctrine

How to approach large-scale refactoring with AI assistance.

---

## 1. Propose Before Implementing

When facing complex refactoring or cascading issues:

1. **Stop and assess** - Don't patch reactively
2. **Propose changes in writing** - Describe what you plan to change and why
3. **Get human approval** - Verify the approach before touching code
4. **Then implement** - With clear direction established

**Why**: Prevents premature patches that add complexity without fixing root causes. Forces structural thinking before coding. Catches wrong assumptions before they become wrong code.

```
❌ Bad: "I see the bug, let me fix it" → patch → new bug → patch → mess
✅ Good: "I see 3 issues. Here's my proposed approach..." → review → implement
```

This is especially important when:
- A fix reveals deeper architectural problems
- Multiple interacting systems are involved
- The original design assumptions may be wrong
- You've already made one fix that didn't work

---

## 2. The Wrap-First Approach

When restructuring a large file with tangled dependencies:

1. **Don't rewrite from scratch** - too risky, loses working code
2. **Don't extract piecemeal** - creates broken intermediate states
3. **Wrap all imperative code in a single function first**

```javascript
// BEFORE: naked top-level code scattered throughout
const x = setup();
doThing();
const y = moreThing();

// AFTER: wrapped in main()
main();

function main() {
  const x = setup();
  doThing();
  const y = moreThing();
}
```

This creates a clean boundary. All nested functions become closures with access to local state. The file now has structure: `imports → entry point → main → helpers`.

---

## 3. Extract to Temp File, Not In-Place

When reorganizing function order or extracting modules:

1. Create a new temp file with your ideal structure (stubbed)
2. Copy functions one-by-one from original into correct sections
3. Syntax-check after each major section
4. Replace original only when complete and tested

**Why**: Avoids broken intermediate states. Original stays runnable throughout.

---

## 4. State Centralization Before Extraction

Before extracting modules, centralize scattered state:

1. Find all `let` declarations at module/function scope
2. Move to a single state object or file
3. Update all references to use centralized state
4. Now extraction is safe - dependencies are explicit

```javascript
// BEFORE: scattered state
let count = 0;
let isActive = false;
let lastUpdate = null;

// AFTER: centralized
const state = {
  count: 0,
  isActive: false,
  lastUpdate: null
};
```

---

## 5. Feature Flags for Optional Complexity

When code has optional features that add complexity:

1. Add feature flags defaulting to OFF
2. Wrap optional code in flag checks
3. Extract to separate modules
4. Main code path stays clean

```javascript
// In state
enableUnblockSystem: false,
enableComplianceSystem: false,

// In code
if (state.enableUnblockSystem) {
  unblock.check();
}
```

---

## 6. Module Extraction Pattern

When extracting to a module:

1. Create module with `init(context)` function
2. Pass only what's needed via context object
3. Keep stub functions in original that delegate to module
4. Module manages its own internal state

```javascript
// lib/feature.js
let ctx = null;
function init(context) { ctx = context; }
function doThing() { ctx.logDebug('doing'); }
module.exports = { init, doThing };

// main file
const feature = require('./lib/feature');
feature.init({ logDebug, state });
function doThing() { feature.doThing(); } // stub
```

---

## 7. Dead Code Identification

Signs of dead code during refactoring:

- State variables only written, never read
- Functions defined but never called (grep for usage)
- Commented-out code with "DISABLED" or "TODO"
- Feature flags that are always false
- Conditional branches that can never execute

**Delete aggressively** - version control preserves history.

---

## 8. Verification Checkpoints

After each refactoring step:

1. `node -c file.js` - syntax check
2. Quick smoke test - does it start?
3. Run existing tests
4. Commit with descriptive message

Don't batch too many changes before verifying.
