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

The long-term product direction combines:
- **AI Pass Workspace** — a company-controlled AI work surface
- **AI Pass Guardrails** — protection when employees use external AI tools directly
- **AI Pass Admin** — policies, activity, devices, destinations

See `docs/PRODUCT_DIRECTION.md` for product intent.

## Read the relevant source of truth

Read only what the task needs:

- Build phase scope/gates: `README.md`
- Product direction: `docs/PRODUCT_DIRECTION.md`
- Service boundaries/data flow: `docs/ARCHITECTURE.md`
- Any UI/UX work: `docs/DESIGN_REFINEMENT_V2.md` first, then `docs/DESIGN_SYSTEM.md`
- Visual reference intent: `docs/UI_REFERENCE_BOARD.md`
- Persistent reference-image instructions: `docs/design-references/README.md`
- Previous observed implementation evidence: `docs/BUILD_LOG.md`

For UI work, `docs/DESIGN_REFINEMENT_V2.md` supersedes conflicting older visual guidance.

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

Do not stop merely to report intermediate progress. Continue until the requested phase passes, fails, or there is a genuine blocker that requires user input.

Use bounded commands. Never leave dev servers, watchers, package queries, interactive prompts, or subprocesses indefinitely blocking the active shell. Temporary servers used for verification must have explicit lifecycle/cleanup.

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

For UI tasks, follow in this priority order:
1. `docs/DESIGN_REFINEMENT_V2.md`
2. user-supplied reference images, especially `docs/design-references/primary-glass-dashboard.png` when present
3. `docs/DESIGN_SYSTEM.md`
4. `docs/UI_REFERENCE_BOARD.md`

Critical constraints:
- **light-first** premium glassmorphism is mandatory
- the previous dark/graphite/“Orbit” direction is retired
- the current purple/lavender AI-SaaS direction is also rejected
- no purple/lilac/indigo product accent, purple icon tiles, or purple send control
- use warm-neutral material with restrained orange/copper brand signal unless explicitly changed by the user
- no robot, shield+sparkle, magic-wand, or generic AI-star branding
- use Instrument Sans by default for the refined UI; Manrope is the preferred fallback
- prefer a narrow detached floating navigation rail over a permanent ChatGPT-style wide sidebar
- prefer one dominant glass application plane with readable inner surfaces over repeated identical white cards
- visible frosted-glass material must use blur + translucency + edge highlight + soft shadow
- background must have meaningful tonal/light variation so the glass reads clearly
- no default shadcn/component-library visual appearance
- no dark security console / neon / cyberpunk styling
- status uses icon + label + semantic color
- keyboard/focus states are required
- support `prefers-reduced-motion`
- animations must clarify state/material changes, not decorate continuously
- visible employee chat surface is **AI Pass Workspace**, not “Orbit”
- do not add real AI-provider/model integrations unless explicitly requested by a later phase

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
- `docs/PRODUCT_DIRECTION.md` when product scope/direction changes
- `docs/ARCHITECTURE.md` when service/data boundaries change
- `docs/DESIGN_REFINEMENT_V2.md` for current high-priority visual corrections
- `docs/DESIGN_SYSTEM.md` when durable UI rules/tokens change
- `docs/UI_REFERENCE_BOARD.md` when durable visual references change
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
