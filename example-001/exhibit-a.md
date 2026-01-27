• I pulled logs and doctrine. Consulted: .ai-lint/doctrine/architecture.md, .ai-lint/doctrine/debugging.md, .ai-lint/doctrine/languages/javascript.md, .ai-lint/rejects/architecture.md, .ai-lint/rejects/
  languages/javascript.md.

  What the logs show (root cause)

  - readiness-loop blocks agent.ready while humanSpeaking (computeSnapshot uses !humanSpeaking), so if you start typing during “Requesting consent”, the agent’s reply never triggers agent.ready.
  - If you hit Enter before that capture happens, handleEnterKey() clears the shadow (clearShadowTerm('human-enter')) which discards the agent’s response. That’s exactly how you can lose a valid email
    block and never reach “Handoff connected”.
  - Evidence lines: [HANDOFF-TRACE] setWaitingForAgent ... reason=human-enter and [READY] ... reason=human-enter with handoffScene=WAITING_RESPONSE while coreHuman=ABSENT, plus the guard !humanSpeaking
    in readiness-loop.

  So: “human typing while waiting for agent response” is currently blocking agent-ready and erasing the response if Enter is pressed.

  AI Lint alignment

  - This is an implicit state machine problem (AR‑R6). The system is actually in a “WAITING_FOR_AGENT_RESPONSE_IGNORING_HUMAN” state, but we don’t name it, so it leaks into timing bugs.
  - Also hits “Single Source of Truth” and “Parallel data pipelines” risk if we start capturing vs clearing different buffers (AR‑R2.1, JS‑25).
  - The fix needs explicit state ownership (Architecture §6) and an enum over booleans (JS‑21) instead of ad‑hoc checks.

  Proposed fix (minimal + explicit, no hidden branch soup)

  1. Add a single explicit input gate state (enum) owned by core/input:
      - humanCommitMode: 'NORMAL' | 'DEFER_UNTIL_AGENT_READY'
  2. When handoff scene is WAITING_RESPONSE or any injected prompt is awaiting an agent reply, set mode to DEFER_UNTIL_AGENT_READY.
  3. Input handling:
      - Allow typing freely.
      - On Enter while in defer mode, do not commit, do not clear shadow. Instead set pendingHumanCommit = buffer, show “Waiting for agent response…” and return.
  4. On agent.ready (after capture and handoff processing), if pendingHumanCommit exists:
      - Commit it, then clear shadow, then set waiting for agent. This preserves order without losing logical state.

  Alternative (more invasive): allow agentReadySignal even when human is speaking, but only while core.waitingForAgent or handoffScene.isWaitingResponse(). This solves the suppression but still risks the
  “clearShadow on Enter” problem, so I’d still gate commits.

  About the queued phase2 prompts:

  - Agree with you. On SUCCESS, clear the handoff injection queue so late phase2/reminders don’t fire.

  About “handoff injections saved as agent turns”

  - Yes, that was real. The edge was recordInjectionAsHumanTurn() using shouldForceCommit() to save handoff|reminder|handshake as agent. That was logged and visible in history (e.g., reminder prompt
    saved in .../00017-agent.txt). It’s now removed.

  If you want me to implement the “DEFER_UNTIL_AGENT_READY” path, say the word and I’ll do it in a minimal diff consistent with doctrine.
