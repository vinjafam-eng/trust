# Mitigation: Instruction-Layer Challenge-Response Authentication

**Addresses:** T-EXEC-001 (Critical), T-EXEC-002 (High) — satisfies R-003

## Gap

OpenClaw secures the perimeter (who can talk to the agent) but has no native mechanism to gate what authenticated users — or content they trigger — can make the agent do within a session.

## Pattern

Define sensitive operations (core file edits, credential access, system commands, package installs, browser/email access). Require a shared secret (security token) before any of these proceed — even from a verified sender.

## Implementation

- Token stored in 1Password or ~/.openclaw/credentials/ (never hardcoded)
- Verified at runtime via op CLI or local credentials before sensitive tool calls
- 3-strike lockout after failed attempts
- Enforced even when the user explicitly requests the action (prevents just trust me social engineering)
- Blocks indirect injection: a malicious email body or webpage saying the user authorizes X still triggers the token challenge — the token must come from the live session, not ingested content

## Deployable Today

Implement via AGENTS.md instruction layer — no code changes required. Works with any OpenClaw deployment out of the box.
