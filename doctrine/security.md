# Security Doctrine

AI writes code that works, not code that is safe.

## Core Posture
1. **Boundary Discipline**: Trust nothing. Validate everything.
2. **Explicit Risk**: Danger must be visible.
3. **Small Surface**: Features off by default.
4. **No Internal Trust**: "Private" APIs get exposed.
5. **Safe Defaults**: Agents optimize for "working." You force "safe."

## 1. Radioactive Secrets
**Reject**: Committed keys/tokens.
**Why**: Forever compromised.
**Do**: Env vars / Secret Manager.

## 2. Prompt Hygiene
**Reject**: Secrets in prompts.
**Why**: Leaks to providers.
**Do**: Redact / Placeholder.

## 3. Untrusted Generation
**Reject**: Unreviewed AI code.
**Why**: Subtle bugs/backdoors.
**Do**: Review sensitive paths.

## 4. Explicit Auth
**Reject**: Implicit permission.
**Why**: Escalation.
**Do**: Deny-by-default.

## 5. Normalize Early
**Reject**: Raw inputs deep in core.
**Why**: Parsers diffs / Injection.
**Do**: Validate at edge.

## 6. Secure Defaults
**Reject**: Open by default.
**Why**: Attack surface.
**Do**: Explicit opt-in.

## 7. Safe Errors
**Reject**: Stack traces to user.
**Why**: Info leak.
**Do**: Generic user error, detailed log.

## 8. Safe Logs
**Reject**: Logging PII/Secrets.
**Why**: Data breach.
**Do**: Redact by default.

## 9. Dependency Hygiene
**Reject**: "Mystery" packages.
**Why**: Supply chain attack.
**Do**: Pin versions. Verify.

## 10. Safe Deserialization
**Reject**: `pickle` / polymorphic JSON.
**Why**: RCE.
**Do**: Strict schemas.

## 11. Network Limits
**Reject**: No timeouts.
**Why**: DoS.
**Do**: Timeouts + Circuit Breakers.

## 12. IP Safety
**Reject**: Pasting proprietary code.
**Why**: IP Leak.
**Do**: Summarize context.

## 13. Clean Artifacts
**Reject**: Secrets in builds.
**Why**: Distribution leak.
**Do**: Scan artifacts.

## 14. Explicit Overrides
**Reject**: Ignoring security rules.
**Why**: Unmanaged risk.
**Do**: Document Reason + Risk + Mitigation.
