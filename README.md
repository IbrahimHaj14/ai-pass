# AI Pass — Local Demo Build Plan

> Working title: **AI Pass**
>
> Goal: build a polished local demonstration of a company-controlled AI safety layer that can detect sensitive information before it is sent to an AI service, allow safe requests, protect/redact sensitive requests, block restricted requests, and report privacy-safe event metadata to an admin dashboard.

This repository is deliberately a **local demo first**. It must prove the product interaction and security model before enterprise deployment, cloud hosting, Intune/Jamf, SSO, endpoint agents, compliance automation, or multi-tenancy are attempted.

---

## 1. Demo thesis

The complete v0 story must be understandable in under two minutes:

1. An employee uses a chat-style AI page.
2. AI Pass watches the send action locally.
3. AI Pass returns one of three outcomes:
   - **GREEN — Safe:** send normally.
   - **YELLOW — Protect:** sensitive information was detected; offer a protected/redacted version.
   - **RED — Block:** restricted information must not leave the device.
4. The user gets a clear, calm explanation and a useful next action.
5. Only privacy-safe metadata is reported to the admin dashboard.
6. The dashboard shows AI activity, policy outcomes, protected-device status, and editable company rules.

The product should feel like a **helpful AI permission layer**, not an intrusive monitoring tool.

---

## 2. Core product principles

### 2.1 Protect without collecting
Raw prompts, raw document content, redaction mappings, passwords, API keys, Civil IDs, account numbers, and other detected secrets must remain on the employee device.

The backend receives only event metadata such as:

```json
{
  "device_id": "demo-device-01",
  "service": "demo-ai",
  "risk": "yellow",
  "detections": ["PERSONAL_IDENTIFIER", "IBAN"],
  "action": "redacted",
  "policy_version": "demo-1",
  "timestamp": "..."
}
```

### 2.2 Help before blocking
Where practical, AI Pass should offer a safe route rather than simply preventing work.

Examples:

- safe content → send
- personal data → redact and send
- confidential information → use approved AI / protect if policy allows
- restricted information → block

### 2.3 Deterministic first
The first demo uses deterministic detection:

- regex/pattern detectors
- company-defined terms
- document labels/markers
- destination policy
- simple severity rules

Do **not** add an LLM to the classifier in the initial demo. The first version must be predictable, explainable, testable, and able to run locally.

### 2.4 Design is part of the product
The demo must look like a serious modern SaaS/security product. It is not acceptable to ship a generic white dashboard or raw component-library defaults. See `docs/DESIGN_SYSTEM.md`.

---

## 3. Planned repository structure

```text
ai-pass/
├── AGENTS.md
├── README.md
├── package.json
├── pnpm-workspace.yaml
├── apps/
│   ├── admin/                 # React/Vite admin portal
│   ├── demo-chat/             # Stable local AI-chat test surface
│   └── extension/             # WXT + React browser extension
├── packages/
│   ├── policy-engine/         # pure TS, local-only classification/redaction
│   ├── ui/                    # shared design tokens/components
│   └── test-fixtures/         # synthetic safe/sensitive demo fixtures only
├── services/
│   └── api/                   # FastAPI + SQLite; metadata only
├── docs/
│   ├── ARCHITECTURE.md
│   ├── DESIGN_SYSTEM.md
│   └── BUILD_LOG.md           # created/maintained during implementation
├── e2e/
│   └── ...
└── scripts/
    └── ...
```

### Recommended stack

- Node.js: current supported LTS
- Package manager: `pnpm`
- Browser extension: **WXT + React + TypeScript**, Manifest V3
- Admin: React + TypeScript + Vite
- Demo chat: React + TypeScript + Vite
- UI primitives: Radix Primitives where useful
- Icons: Lucide or equivalent restrained outline set
- Motion: CSS transitions first; Motion/React animation library only where it materially improves interaction
- Unit tests: Vitest
- Browser E2E: Playwright
- API: Python + FastAPI + Pydantic
- Local DB: SQLite
- API tests: pytest
- Fonts: Geist as the default product typeface unless a clearly stronger equivalent is selected during Phase 1

WXT is preferred because it supports modern Manifest V3 browser-extension development, Vite, TypeScript, React integration, file-based entrypoints, and multiple Chromium targets from one codebase.

---

# 4. Phase rules

Implementation is **strictly phase-gated**.

Codex must not start the next phase because the current phase “looks complete.”

Each phase ends with:

1. build/lint/typecheck as applicable
2. unit/integration tests for the phase
3. manual or browser verification where applicable
4. acceptance evidence
5. update to `docs/BUILD_LOG.md`
6. an explicit `PASS_PHASE_<N>` or `FAIL_PHASE_<N>` result

If a gate fails, fix the current phase. Do not continue.

---

# 5. Phased implementation

## Phase 0 — Repository foundation

### Purpose
Create a clean, reproducible monorepo with no product behavior yet.

### Build
- initialize Git repository/project structure
- configure `pnpm` workspace
- scaffold:
  - `apps/admin`
  - `apps/demo-chat`
  - `apps/extension`
  - `packages/policy-engine`
  - `packages/ui`
  - `packages/test-fixtures`
  - `services/api`
- configure TypeScript
- configure formatting/linting with one consistent toolchain
- configure Vitest
- configure Playwright
- configure Python environment for FastAPI/pytest
- add root scripts for common commands
- create `docs/BUILD_LOG.md`
- add `.env.example`; no real secrets
- ensure extension requests only permissions needed by the demo
- add a root development command or documented small set of commands

### Required root commands
Equivalent commands are acceptable, but the repo must expose simple equivalents of:

```bash
pnpm install
pnpm dev
pnpm build
pnpm typecheck
pnpm test
pnpm test:e2e
```

API setup/run/test must be documented.

### Non-goals
- no classifier
- no backend event persistence
- no extension interception
- no real AI sites

### Gate
PASS only when:
- all apps can start
- TypeScript compiles
- placeholder tests pass
- FastAPI health placeholder can run locally
- no dependency/security warnings that materially block the demo
- repository structure matches `docs/ARCHITECTURE.md`

Output: `PASS_PHASE_0`

---

## Phase 1 — Design foundation and polished static shells

### Purpose
Establish the visual system **before** feature code causes inconsistent UI.

### Build
Implement `docs/DESIGN_SYSTEM.md` as reusable code.

Create in `packages/ui`:
- color tokens
- spacing tokens
- radii
- shadow/elevation tokens
- glass/material tokens
- typography scale
- motion tokens
- status components
- buttons
- badges/pills
- cards/panels
- tooltip/popover/dialog primitives
- table/list row primitives
- empty/loading/error states

Create polished static shells for:

### Admin
- sidebar/navigation
- top context bar
- overview page
- metrics row
- recent activity surface
- protected devices surface
- policy page shell

### Demo chat
- neutral AI chat interface
- message list
- composer
- attachment affordance
- subtle AI Pass protected-state indicator

### Extension
- toolbar popup shell
- intervention-card static states:
  - green
  - yellow
  - red
  - scanning

### UI requirements
- no generic component-library look
- no pure-white-on-white dashboard
- no cyberpunk/neon security cliché
- restrained glassmorphism
- meaningful depth
- purposeful animations
- keyboard focus states
- reduced-motion support
- status is never encoded by color alone

### Gate
PASS only when:
- desktop UI is visually coherent at 1280, 1440 and 1600 px widths
- no overflow/clipping at 1024 px
- extension card works at narrow browser-overlay widths
- all four status states are visibly distinct
- screenshots are captured for review
- visual hierarchy follows `docs/DESIGN_SYSTEM.md`
- reduced-motion mode removes nonessential movement

Output: `PASS_PHASE_1`

---

## Phase 2 — Local deterministic policy engine

### Purpose
Build the security decision engine independently of the extension and backend.

### Location
`packages/policy-engine`

### Required characteristics
- pure TypeScript
- deterministic
- synchronous where practical
- no HTTP/network calls
- no LLM/API dependency
- no persistence of raw content
- unit-testable as a pure library

### Initial detector categories
Use synthetic/demo-grade detection with clear naming. Do not claim regulatory-grade identity validation.

At minimum:
- `EMAIL`
- `PHONE`
- `IBAN`
- `PERSONAL_IDENTIFIER_GCC_DEMO`
- `API_KEY_OR_TOKEN`
- `PASSWORD_LIKE_SECRET`
- `COMPANY_CONFIDENTIAL_TERM`
- `RESTRICTED_MARKER`

Support company-defined terms, e.g.:

```text
Project Falcon -> RESTRICTED
Client Atlas -> CONFIDENTIAL
Hotel Acquisition 2027 -> RESTRICTED
```

### Decision model

At minimum:

```text
ALLOW
PROTECT
BLOCK
```

Every result must include:
- decision
- severity
- detector IDs
- matched span locations where needed for local redaction
- human-readable reason key
- policy rule ID

### Tests
Include:
- safe marketing prompt
- email/phone
- IBAN
- synthetic GCC identifier
- fake API token
- company confidential term
- restricted marker
- overlapping matches
- false-positive resistance fixtures
- mixed Arabic/English/Arabizi fixtures where deterministic rules apply

### Gate
PASS only when:
- engine tests pass
- engine has zero network dependency
- engine exposes no logging of raw values
- deterministic fixtures always produce identical results

Output: `PASS_PHASE_2`

---

## Phase 3 — Extension interception on the local demo chat

### Purpose
Prove that AI Pass can sit in the employee workflow.

### Scope
The extension supports **only the local demo-chat origin** in this phase.

### Build
- WXT Manifest V3 extension
- content script detects the demo chat composer
- intercept:
  - Send button
  - Enter-to-send
- read outgoing text locally
- call local policy engine
- show a compact AI Pass intervention UI

### Behavior

#### GREEN
- brief, nonintrusive “Safe” acknowledgement
- send proceeds

#### YELLOW
- send pauses
- show detections by category, never echo the sensitive value
- primary action: `Send safely`
- secondary action: `Cancel`

#### RED
- send is prevented
- clear reason
- no dangerous override in v0
- useful next action such as `Cancel` or `View policy`

### Important
No event API yet. The entire phase should work offline.

### Gate
PASS only when:
- mouse send is intercepted
- keyboard send is intercepted
- safe prompt sends
- yellow prompt cannot leave until the user selects an action
- red prompt never reaches demo-chat send handler
- UI does not break the host page
- extension does not require broad unrelated browser permissions

Output: `PASS_PHASE_3`

---

## Phase 4 — SafeSend redaction and local rehydration demo

### Purpose
Demonstrate that security can preserve productivity.

### Build
For YELLOW decisions:
- replace sensitive spans with stable placeholders
- examples:
  - `PERSON_1`
  - `EMAIL_1`
  - `IBAN_1`
  - `ACCOUNT_1`
  - `[REDACTED_IDENTIFIER]`
- preserve readability of the prompt
- keep replacement mapping only in extension memory for the active interaction
- send protected prompt to demo chat
- optionally demonstrate response rehydration where deterministic and safe

### Rules
- mappings never go to FastAPI
- raw prompt never goes to FastAPI
- browser console must not log sensitive fixture values
- cancellation clears temporary mappings
- refresh/navigation clears ephemeral mappings unless explicitly needed for current demo flow

### Gate
Use a sentinel string such as:

```text
TOP_SECRET_SENTINEL_92841
```

PASS only when:
- the protected outbound message does not contain the original sensitive value
- the local redaction works
- the sentinel does not appear in server/network logs
- the redaction mapping is not persisted to backend/storage
- tests cover multiple values and repeated occurrences

Output: `PASS_PHASE_4`

---

## Phase 5 — Metadata API, local SQLite persistence, policies and heartbeat

### Purpose
Add the minimum backend needed for admin visibility while preserving the privacy boundary.

### API
FastAPI endpoints should remain narrow.

Suggested endpoints:

```text
GET  /health
GET  /api/policies
PUT  /api/policies
POST /api/events
GET  /api/events
POST /api/devices/heartbeat
GET  /api/devices
```

Exact naming can change if the architecture remains clear.

### Event schema
Use an allowlist schema. Unknown fields are rejected.

The API must **not** accept:
- prompt
- raw_text
- raw_content
- document bytes
- redaction map
- detected secret value

### Policies
Store:
- policy version
- destination rules
- detector severity mapping
- custom confidential/restricted terms for the demo

### Device heartbeat
Extension periodically reports:
- opaque demo device ID
- extension version
- policy version
- last seen
- browser
- protection status

No invasive hardware fingerprinting.

### SQLite
Store:
- events
- policies
- demo devices/heartbeats

### Gate
PASS only when:
- strict Pydantic models reject raw-content-like fields
- SQLite persists metadata
- extension can retrieve policy
- extension can post privacy-safe event
- heartbeat updates last-seen state
- API tests pass
- sentinel raw content never reaches API

Output: `PASS_PHASE_5`

---

## Phase 6 — Functional admin dashboard

### Purpose
Turn backend metadata into a polished management experience.

### Overview page
Show:
- protected devices
- total AI interactions
- protected/redacted
- blocked
- awaiting attention if implemented
- recent events
- AI service distribution

Keep the dashboard calm. Do not fill it with charts simply because data exists.

### Devices page
At minimum:
- device ID / friendly demo name
- status
- browser
- extension version
- policy version
- last seen
- states:
  - Protected
  - Outdated
  - Offline

### Policies page
Allow admin to edit:
- default handling by sensitivity
- custom company terms
- approved/unapproved destination state
- term severity

Policy changes should increment or update a policy version and be picked up by the extension.

### Activity page
Display:
- timestamp
- device/user demo identity
- destination
- decision
- detection categories
- action

Never display raw sensitive prompt content.

### Gate
PASS only when:
- dashboard uses live API data, not hard-coded metrics
- custom term can be created in admin and later affects extension behavior
- protected-device heartbeat is visible
- empty/loading/error states are polished
- all statuses have text/icon in addition to color
- UI passes the Phase 1 visual standard

Output: `PASS_PHASE_6`

---

## Phase 7 — Destination-aware permissions

### Purpose
Move from generic DLP toward the core “AI permission layer” idea.

### Demo destinations
The demo chat should be able to simulate at least:

- `Company AI` — APPROVED
- `External AI` — LIMITED
- `Unapproved AI` — BLOCKED or PUBLIC_ONLY

The extension/policy engine receives destination context.

### Example policy

```text
PUBLIC       -> approved/external allowed
INTERNAL     -> company AI only
CONFIDENTIAL -> company AI or protected route
RESTRICTED   -> no external AI
PII          -> protected route where policy permits
```

### UI
Before or during intervention, make it obvious:
- what service/account is being used
- whether it is approved
- why the current data/service combination matters

### Gate
PASS only when the **same prompt** can produce different outcomes based on destination policy.

Example:
- internal prompt + Company AI -> ALLOW
- same prompt + External AI -> BLOCK or PROTECT according to configured rule

Output: `PASS_PHASE_7`

---

## Phase 8 — Local document inspection

### Purpose
Demonstrate file-aware protection without uploading files to the backend.

### Initial file support
Prefer a limited, reliable set:
- `.txt`
- `.csv`
- text-based `.pdf` if local PDF extraction is stable

Do not use OCR in v0.

### Behavior
Before demo-chat accepts an attachment:
- inspect locally
- classify
- detect restricted markers/custom terms/selected PII patterns
- apply ALLOW / PROTECT / BLOCK
- only metadata may be recorded

For YELLOW file behavior, it is acceptable for v0 to:
- warn and block direct upload
- offer a protected text extraction if safe and technically reasonable

Do not silently mutate binary files.

### Gate
PASS only when:
- restricted sample file is blocked
- safe sample file proceeds
- file bytes never reach FastAPI
- no raw extracted text reaches FastAPI
- unsupported/scanned PDFs produce a clear, safe message rather than guessing

Output: `PASS_PHASE_8`

---

## Phase 9 — Demo hardening, E2E, security verification and presentation polish

### Purpose
Produce a demo that can be shown to another person without developer explanation.

### Required demo scenarios

#### Scenario A — GREEN
Prompt:
> Write a short announcement for our office opening.

Expected:
- Green
- sends normally
- metadata event visible

#### Scenario B — YELLOW
Synthetic prompt containing:
- fake person name
- fake GCC-style identifier
- fake IBAN/account detail

Expected:
- Yellow
- user chooses `Send safely`
- prompt is locally redacted
- protected message sends
- admin sees category metadata only

#### Scenario C — RED
Prompt/document references a company policy term such as:
> Project Falcon acquisition targets

Expected:
- Red
- no send
- useful explanation
- block event visible

#### Scenario D — DESTINATION
Same internal content:
- Company AI -> permitted
- External AI -> denied/protected

### E2E tests
Playwright must cover:
- send button
- enter-to-send
- SafeSend
- blocking
- policy update
- heartbeat
- dashboard event appearance
- destination-aware rule
- one document flow

### Security assertions
Automated test must fail if known sentinel sensitive strings appear in:
- API request bodies
- SQLite event table
- normal server logs
- persisted browser storage used by the demo

### UX polish
- loading/skeleton states
- empty states
- errors with recovery
- motion/reduced motion
- keyboard navigation
- responsive admin
- extension overlay position resilience
- no console errors in happy-path demo

### Deliverables
- working local extension
- working demo chat
- working FastAPI service
- working admin portal
- SQLite demo DB created automatically
- automated unit/integration/E2E test suite
- synthetic demo fixtures
- short run instructions
- demo script in README
- architecture/security docs
- packaged extension build artifact

### Gate
PASS only when all required demo scenarios pass from a clean setup.

Output: `PASS_PHASE_9_DEMO_COMPLETE`

---

# 6. Explicitly out of scope for the local demo

Do not implement these until the local demo is accepted:

- Intune deployment
- Jamf/Kandji
- force-install enterprise browser policies
- Microsoft Entra ID / SSO
- Google Workspace SSO
- multi-tenant SaaS
- cloud production deployment
- UAE/Saudi hosting
- production compliance reports
- SIEM integration
- SOC 2 / ISO workflows
- endpoint Windows/macOS agent
- MCP monitoring
- IDE monitoring
- real ChatGPT/Claude/Gemini adapters
- billing
- mobile
- remote LLM classification
- employee surveillance features
- keylogging/screen capture
- production-grade insider threat detection

---

# 7. Demo run target

The end-state local workflow should be approximately:

```bash
# terminal 1
pnpm dev

# terminal 2
cd services/api
# activate environment
uvicorn app.main:app --reload
```

Then:

- demo chat opens locally
- admin opens locally
- extension is loaded unpacked in Chrome
- extension reports Protected
- three core demo scenarios work

Exact commands may be refined during Phase 0.

---

# 8. Acceptance philosophy

A phase is not complete because:
- code was generated
- files exist
- Codex says tests should pass
- the UI looks fine in source
- one happy path worked

A phase is complete only with **observed evidence**.

For each phase Codex reports:

```text
PHASE:
STATUS:
FILES CHANGED:
COMMANDS RUN:
TEST RESULTS:
MANUAL/BROWSER VERIFICATION:
SECURITY/PRIVACY CHECKS:
KNOWN LIMITATIONS:
NEXT PHASE NOT STARTED:
```

Codex must stop after the requested phase.

---

# 9. Future path after the demo

Only after `PASS_PHASE_9_DEMO_COMPLETE`, evaluate:

1. real ChatGPT/Claude/Gemini web adapters
2. Microsoft 365 / Entra integration
3. managed Chrome/Edge deployment
4. central policy signing
5. real identity/device inventory
6. GCC identifier/language classifier research
7. endpoint agent
8. enterprise tenancy and cloud deployment
9. audit/compliance evidence
10. AI app directory and approval workflow
11. agent/MCP permissions

The demo is successful if it makes these future investments worth pursuing.
