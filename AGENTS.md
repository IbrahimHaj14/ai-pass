# AGENTS.md — AI Pass

This file contains durable repository instructions for coding agents. Keep it concise. Detailed product, architecture, and design rules live in the linked docs.

## Product goal

AI Pass is a local-first demo of an enterprise AI permission/safety layer.

The core user flow is:

```text
employee sends to AI
-> inspect locally
-> ALLOW / PROTECT / BLOCK
-> send or stop
-> report privacy-safe metadata
-> admin visibility
```

The product should help employees use AI safely, not act like employee-surveillance software.

## Read the relevant source of truth

Read only what the task needs:

- Build phase scope/gates: `README.md`
- Service boundaries, data flow, repository structure: `docs/ARCHITECTURE.md`
- Any UI/UX work: `docs/DESIGN_SYSTEM.md`
- Previous observed implementation evidence: `docs/BUILD_LOG.md`

Do not reread every document for trivial edits.

## Phase discipline

When asked to implement a numbered phase:

1. implement only that phase
2. do not begin later phases
3. run the required checks
4. record observed evidence in `docs/BUILD_LOG.md`
5. report `PASS_PHASE_<N>` or `FAIL_PHASE_<N>`

A phase does not pass on code inspection alone.

Do not silently broaden scope to “help future phases.”

## Privacy boundary — non-negotiable

Raw employee content must remain local to the extension/device in the demo.

Never send or persist to the API:
- raw prompts
- raw file/document text
- document bytes
- passwords
- API keys/tokens
- PII values
- company confidential values
- redaction mappings

Backend events use an explicit metadata allowlist.

If a proposed implementation violates this boundary, stop and redesign it.

Do not add telemetry/analytics SDKs that could capture prompt content.

## Local policy engine

`packages/policy-engine` must remain:
- deterministic
- locally executable
- network independent
- testable as a pure library

Do not introduce an LLM/classification API unless a later approved phase explicitly requires it.

Never log detected sensitive values.

## Security implementation rules

- request the least browser permissions possible
- do not add keylogging, screen capture, clipboard history, or unrelated browsing telemetry
- validate all API inputs
- reject unknown/raw-content fields in event schemas
- use synthetic sensitive fixtures only
- never commit real secrets or real personal data
- keep `.env` secrets out of Git
- treat demo identifiers as explicitly synthetic

For a security-sensitive change, add or update a regression test.

## UI rules

For UI tasks, follow `docs/DESIGN_SYSTEM.md`.

Critical constraints:
- polished, modern, deliberate UI
- no generic white SaaS dashboard
- no default shadcn/component-library visual appearance
- no neon/cyberpunk security cliché
- glass is selective, not everywhere
- status uses icon + label + color
- keyboard/focus states are required
- support `prefers-reduced-motion`
- animations must clarify state changes, not decorate continuously

Reuse shared tokens/components instead of introducing one-off styling.

## Technical preferences

- TypeScript for browser/client packages
- React for UI
- WXT for the extension
- Vite for admin/demo web apps
- FastAPI/Pydantic for the local API
- SQLite for demo persistence
- Vitest for TS unit tests
- pytest for API tests
- Playwright for browser E2E

Do not replace the stack without a concrete reason tied to the current phase.

## Code quality

Prefer:
- small modules
- explicit names
- typed boundaries
- pure functions for policy logic
- boring, readable security code
- accessible primitives
- comments for security invariants, not obvious syntax

Avoid:
- premature frameworks/services
- unnecessary abstractions
- duplicated policy logic
- large monolithic React components
- magic severity values
- hidden network behavior

## Verification

For changed code, run the narrowest relevant checks plus the phase gate.

Never claim a test passed unless it was actually run.

For UI changes, browser verification is required when the environment permits it.

For privacy-sensitive flows, verify with a known sentinel string that the raw value did not cross the intended boundary.

## External writes

Do not:
- push
- merge
- deploy
- publish extensions
- create cloud resources
- change external accounts

unless the user explicitly asks for that action.

Local commits are fine only when the user's requested workflow calls for them.

## Documentation ownership

Update:
- `README.md` when build phases or run instructions change
- `docs/ARCHITECTURE.md` when service/data boundaries change
- `docs/DESIGN_SYSTEM.md` when durable UI rules/tokens change
- `docs/BUILD_LOG.md` with observed phase evidence

Do not duplicate the same rule across multiple files unless needed for safety.

## Final phase response format

When completing a requested phase, report:

```text
PHASE:
STATUS:
FILES CHANGED:
COMMANDS RUN:
TEST RESULTS:
MANUAL/BROWSER VERIFICATION:
SECURITY/PRIVACY CHECKS:
KNOWN LIMITATIONS:
NEXT PHASE NOT STARTED: yes
```
