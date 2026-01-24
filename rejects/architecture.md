# Architecture Rejects (Preview)

Patterns that work but create unmaintainable systems.

---

## AR-R1: Flag Explosion

**Reject**:
```javascript
let isReady = false;
let wasReady = false;
let isAlmostReady = false;
let readyButWaiting = false;
// 50 more flags...
```

**Why it rots**: Each flag is a boolean dimension. 50 flags = theoretically 2⁵⁰ states. In practice, most combinations are "impossible" but the code doesn't know that. Bugs hide in the impossible corners.

**Instead**: Identify the actual states. Use an enum or state machine. `state = 'IDLE' | 'WORKING' | 'WAITING' | 'ERROR'` is 4 states, not 2⁵⁰.

---

## AR-R2: Parallel Solutions

**Reject**:
```javascript
// Three ways to detect stuck agent, all active:
function checkStuckLegacy() { ... }
function checkStuckV2() { ... }
function checkStuckStaged() { ... }
```

**Why it rots**: Each solution handles different edge cases differently. When they disagree, behavior is undefined. "Which one fired?" becomes unanswerable.

**Instead**: Pick the best one. Delete the others. If the chosen solution lacks features, add them to it—don't resurrect the old ones.

---

## AR-R3: Defensive Code as Core Logic

**Reject**:
```javascript
function processMessage(msg) {
  // 200 lines of "what if agent doesn't comply"
  // 20 lines of actual message processing
  // 150 lines of "what if timing is weird"
}
```

**Why it rots**: The core path is invisible. New developers think the defensive code IS the logic. Defensive code grows without bound because fear is unbounded.

**Instead**: 
```javascript
function processMessage(msg) {
  // 20 lines of core logic
  resilience.handleNonCompliance(msg, this.retry);
  timing.guardAgainstRaces(msg, this.queue);
}
```

---

## AR-R4: The 4000-Line File

**Reject**: Any file over 1000 lines, especially over 2000.

**Why it rots**: 
- "Where is X?" requires Ctrl+F archaeology
- Functions call functions 2000 lines away
- Multiple concerns pretending to be one
- Refactoring is terrifying

**Instead**: Extract by concern, not by arbitrary split.

---

## AR-R5: Interleaved Concerns

**Reject**:
```javascript
function onAgentReady() {
  updateUI();
  checkCompliance();
  maybeUnblock();
  sendEmail();
  trackTiming();
  logMetrics();
  updateUI();  // again
}
```

**Why it rots**: One function does six unrelated things. Change one, break the others. Test one, mock five.

**Instead**: Event-driven or pub/sub. `onAgentReady` emits event. Listeners handle their own concerns independently.

---

## AR-R6: Implicit State Machines

**Reject**: State machine behavior without explicit states:
```javascript
if (isReady && !wasReady && handoffEnabled && !emailSent && turnCounter > 0) {
  // This is actually "FIRST_READY_IN_HANDOFF_MODE"
}
```

**Why it rots**: The state exists but has no name. Same condition appears in 5 places with slight variations. Off-by-one in any flag breaks everything.

**Instead**: Name the state. Check it once. React to transitions.
```javascript
if (state === 'FIRST_READY_IN_HANDOFF') { ... }
```

---

*This is a preview. The full Architecture Rejects includes additional patterns: Timer Soup, God Objects, Comment Archaeology, and Late Extraction.*

*Get the complete rejects at https://ai-lint.dosaygo.com*
