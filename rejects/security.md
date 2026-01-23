# Security Rejects

Non-negotiable security boundaries.

---

## 1. Radioactive Secrets
**Reject**: Committed keys, tokens, or passwords.
**Why**: Forever compromised in git history.
**Do**: Environment variables or Secret Manager.

---

## 2. Secrets in Prompts
**Reject**: Pasting credentials into AI context.
**Why**: Leaks to third-party providers.
**Do**: Redact or use placeholders.

---

## 3. Unreviewed Generation
**Reject**: Committing AI code without reading it.
**Why**: Subtle logic bugs and hallucinations.
**Do**: Human review of every line.

---

## 4. Implicit Auth
**Reject**: Actions without explicit permission checks.
**Why**: Privilege escalation.
**Do**: Deny-by-default.

---

## 5. Deep Raw Inputs
**Reject**: Passing raw strings deep into core logic.
**Why**: Injection attacks (SQLi, XSS).
**Do**: Validate/Parse at the edge.

---

## 6. Open by Default
**Reject**: Public access unless specified private.
**Why**: Accidental exposure.
**Do**: Explicit opt-in for public access.

---

## 7. Stack Traces to User
**Reject**: Leaking internals in error responses.
**Why**: Information disclosure.
**Do**: Generic error messages.

---

## 8. PII Logging
**Reject**: Logging emails, phones, or secrets.
**Why**: Compliance violation (GDPR/CCPA).
**Do**: Redact or hash PII.

---

## 9. Mystery Dependencies
**Reject**: `npm install` without vetting.
**Why**: Supply chain attacks.
**Do**: Pin versions and scan vulnerabilities.

---

## 10. Unsafe Deserialization
**Reject**: `pickle.load` or `ObjectInputStream`.
**Why**: Remote Code Execution (RCE).
**Do**: JSON or strict schema protocols.

---

## 11. No Timeouts
**Reject**: Unbounded network calls.
**Why**: Denial of Service (DoS).
**Do**: Explicit timeouts and circuit breakers.

---

## 12. Copy-Paste IP
**Reject**: Pasting proprietary code into public LLMs.
**Why**: IP Leakage.
**Do**: Summarize or use Enterprise mode.

---

## 13. Dirty Artifacts
**Reject**: Secrets in Docker images/builds.
**Why**: Distributed leaks.
**Do**: Multi-stage builds.

---

## 14. Blanket Rule Suppression on Security Lines
**Reject**: `@ts-ignore`, `# noqa`, or `// NOLINT` on security-relevant code without justification.
**Why**: Suppressions without explanation hide deliberate vulnerabilities or technical debt.
**Do**: If suppression is necessary (e.g., linter false positive on a proven-safe pattern), document why: `# noqa: S101 - assert used for test invariant, not production guard`.

---

## 15. Hardcoded IPs
**Reject**: Hardcoded IP addresses.
**Why**: Fragile. Hard to rotate.
**Do**: DNS or Config.

---

## 16. Environment-Specific Configuration Without Override Path
**Reject**: Configuration that only works in a specific environment (dev/test/prod) with no explicit override mechanism.
**Why**: Works in tests, breaks in prod. AI agents love to hardcode happy-path configs.
**Do**: Every config value must have a documented default AND an override path (env var, config file, CLI flag).
