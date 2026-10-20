# AI Pass — Architecture

## 1. Purpose

This document defines the local demo's technical boundaries and component responsibilities.

The architecture is intentionally small. The demo must prove the AI Pass interaction and privacy model before enterprise deployment complexity is introduced.

---

## 2. System overview

```text
                        ADMIN
                          |
                          v
                 +------------------+
                 |  Admin React UI  |
                 +--------+---------+
                          |
                    metadata/policy
                          |
                          v
                 +------------------+
                 | FastAPI + SQLite |
                 +--------+---------+
                          ^
                          |
                  metadata only
                          |
+-------------------------+-----------------------------+
|                                                       |
| EMPLOYEE BROWSER                                      |
|                                                       |
|  +-------------------+       +---------------------+  |
|  |   Demo AI Chat    |<----->| AI Pass Extension   |  |
|  |   localhost       |       | WXT / React / MV3   |  |
|  +-------------------+       +----------+----------+  |
|                                         |             |
|                                         v             |
|                              +---------------------+  |
|                              | Local Policy Engine |  |
|                              | pure TypeScript     |  |
|                              +---------------------+  |
|                                                       |
+-------------------------------------------------------+
```

### Critical boundary

The backend is an **administration and metadata service**, not a content-inspection service.

Raw prompt/file inspection occurs locally.

---

## 3. Component responsibilities

## 3.1 `apps/extension`

Responsibilities:
- run content scripts on explicitly supported origins
- identify AI composer/send actions
- collect the current outbound content for local analysis
- obtain destination context
- call the local policy engine
- render intervention UI
- perform local redaction
- allow/protect/block the outbound action
- post privacy-safe metadata events
- retrieve policy configuration
- send device heartbeat

Must not:
- forward raw prompt/file content to the API
- persist raw sensitive data
- inspect unrelated sites in v0
- use broad permissions without need

Recommended framework:
- WXT
- React
- TypeScript
- Manifest V3

### Extension internal modules

Suggested shape:

```text
apps/extension/
├── entrypoints/
│   ├── background.ts
│   ├── content.tsx
│   └── popup/
├── src/
│   ├── adapters/
│   │   └── demo-chat.ts
│   ├── intervention/
│   ├── policy/
│   ├── transport/
│   ├── heartbeat/
│   └── storage/
└── ...
```

Keep host-specific DOM logic inside adapters.

Do not mix site-selector logic with policy logic.

---

## 3.2 `packages/policy-engine`

The most security-sensitive package.

Responsibilities:
- normalized input classification
- deterministic detectors
- custom company-term matching
- destination-aware policy evaluation
- severity calculation
- redaction span generation
- protected placeholder generation

Must be usable without:
- DOM
- Chrome APIs
- backend
- network
- database

Conceptual API:

```ts
type PolicyDecision = "ALLOW" | "PROTECT" | "BLOCK"

type Detection = {
  kind: string
  severity: "low" | "medium" | "high" | "critical"
  start: number
  end: number
  ruleId: string
}

type Evaluation = {
  decision: PolicyDecision
  detections: Detection[]
  reasonKeys: string[]
  policyVersion: string
}
```

Raw matched values should not be included in serialized result types.

If local redaction requires values internally, keep them ephemeral and separate from reportable metadata.

---

## 3.3 `apps/demo-chat`

Purpose:
Provide a stable development surface independent of ChatGPT/Claude DOM changes.

Responsibilities:
- realistic chat UX
- normal send behavior
- Enter-to-send
- attachment affordance
- simulated AI response
- configurable destination/account mode:
  - Company AI
  - External AI
  - Unapproved AI

It should expose stable semantic hooks for the extension adapter, preferably via attributes such as:

```html
data-ai-pass-composer
data-ai-pass-send
data-ai-pass-destination
```

This app is a test harness, not a clone of any vendor UI.

---

## 3.4 `services/api`

Responsibilities:
- health
- policy storage/retrieval
- event metadata ingestion/query
- device heartbeat
- admin dashboard data
- SQLite persistence

Must reject unknown fields that could smuggle raw content.

Suggested entities:

```text
Policy
Event
Device
```

### Event fields

Allow:
- event ID
- timestamp
- device ID
- demo user ID/name
- service/destination
- risk/decision
- detection categories
- rule IDs
- action
- policy version
- extension version

Do not allow:
- prompt
- content
- message
- file body
- file text
- secret value
- PII value
- redaction mapping

Use strict Pydantic models.

---

## 3.5 `apps/admin`

Responsibilities:
- overview metrics
- activity list
- protected device status
- policy editing
- company terms
- destination rules

This UI never needs the employee's raw prompt.

The product story should therefore remain credible even if the dashboard database were inspected: it contains security decisions, not the confidential content itself.

---

## 3.6 `packages/ui`

Shared product visual system.

Responsibilities:
- tokens
- typography
- status language
- buttons
- panels
- badges
- navigation patterns
- dialogs/popovers
- accessible interaction states
- motion primitives

Source of truth: `docs/DESIGN_SYSTEM.md`.

Do not force extension UI and admin UI into identical layout patterns; they share a visual language, not necessarily identical component dimensions.

---

## 3.7 `packages/test-fixtures`

Synthetic-only data.

Examples:
- fake emails
- fake phone numbers
- clearly fake IBAN-like strings
- synthetic identifier strings
- fake API token formats
- fake company terms such as `Project Falcon`

Never store real employee/customer data.

---

# 4. Data flows

## 4.1 Safe send

```text
User presses Send
  -> extension adapter captures draft locally
  -> policy-engine.evaluate(...)
  -> ALLOW
  -> extension releases original send
  -> metadata event posted
```

No raw prompt is sent to FastAPI.

---

## 4.2 Protected send

```text
User presses Send
  -> extension captures draft locally
  -> policy-engine evaluates
  -> PROTECT
  -> extension displays intervention
  -> user chooses Send safely
  -> policy-engine/redactor creates protected text
  -> extension sends protected text to demo AI
  -> ephemeral replacement map stays local
  -> metadata event posted
```

The backend sees categories such as `IBAN`, not the IBAN value.

---

## 4.3 Blocked send

```text
User presses Send
  -> extension captures draft locally
  -> policy-engine evaluates
  -> BLOCK
  -> extension prevents host send
  -> intervention explains why
  -> metadata event posted
```

The restricted text never reaches the destination or admin API.

---

## 4.4 Policy sync

```text
Admin updates policy
  -> FastAPI validates/persists
  -> policy version changes
  -> extension polls/retrieves policy
  -> local policy-engine evaluates using new policy
```

For the demo, polling is acceptable.

Production signing/distribution is future work.

---

## 4.5 Device health

```text
Extension starts
  -> sends heartbeat metadata
  -> repeats at conservative interval
  -> API updates last_seen
  -> admin classifies device:
       Protected / Outdated / Offline
```

No invasive device fingerprinting.

---

# 5. Privacy invariants

These are architectural invariants, not optional conventions.

### INV-001
Raw prompts never cross from employee-side inspection into `services/api`.

### INV-002
Raw document content and file bytes never cross into `services/api`.

### INV-003
Redaction mappings are never sent to or persisted by `services/api`.

### INV-004
Backend event schemas reject unknown/raw-content fields.

### INV-005
Sensitive detector matches are reportable by **category and rule ID**, not matched value.

### INV-006
Console/server logging must not contain sensitive fixture values in normal operation.

### INV-007
Only explicitly supported AI surfaces are inspected.

### INV-008
The extension uses least-privilege browser permissions.

Every phase that touches these invariants needs regression tests.

---

# 6. Policy model

A simple policy model is enough for v0.

## Data classes

```text
PUBLIC
INTERNAL
PERSONAL
CONFIDENTIAL
RESTRICTED
SECRET
```

## Destination classes

```text
APPROVED_ENTERPRISE
EXTERNAL_LIMITED
UNAPPROVED
```

## Actions

```text
ALLOW
PROTECT
BLOCK
```

Illustrative matrix:

| Data class | Approved enterprise | External limited | Unapproved |
|---|---:|---:|---:|
| Public | Allow | Allow | Allow/Policy |
| Internal | Allow | Block/Protect | Block |
| Personal | Protect/Allow by policy | Protect | Block |
| Confidential | Allow/Protect | Block | Block |
| Restricted | Block by default | Block | Block |
| Secret | Block | Block | Block |

The matrix is configurable; hard-code only safe demo defaults.

---

# 7. Local redaction model

Redaction is span-based.

Example:

```text
Before:
Email Sarah at sarah@example.com about account 1234.

After:
Email PERSON_1 at EMAIL_1 about ACCOUNT_1.
```

Requirements:
- deterministic placeholder naming within one interaction
- preserve punctuation/readability
- handle repeated identical values consistently
- handle overlapping spans predictably
- ephemeral mapping
- avoid sending mapping to backend

Rehydration is a demo convenience, not a security requirement. If implemented, it occurs only locally.

---

# 8. Extension adapter model

Do not build one giant content script.

Define a small adapter interface such as:

```ts
interface AiSurfaceAdapter {
  id: string
  matches(): boolean
  getDraft(): string | null
  getDestinationContext(): DestinationContext
  interceptSend(handler: SendHandler): Cleanup
  sendProtected(text: string): Promise<void>
}
```

Phase 3 implements `DemoChatAdapter`.

Real vendor adapters belong after the local demo is accepted.

---

# 9. Testing architecture

## Unit
`packages/policy-engine`
- detector behavior
- classification
- policy resolution
- redaction
- destination matrix

## API
`services/api`
- strict schemas
- event persistence
- policy CRUD
- heartbeat
- raw-content rejection

## Component
- intervention states
- policy controls
- status elements

## E2E
Playwright with:
- demo chat
- loaded extension where supported by test setup
- local API
- admin

## Sentinel privacy test
Use known synthetic secrets and assert they do not appear in:
- API request capture
- DB
- server log
- persisted extension storage

---

# 10. Failure behavior

Prefer safe, understandable behavior.

Examples:

### Policy API unavailable
Extension uses last-known valid policy if available.

For the early demo, if no valid policy exists:
- fail safe for sensitive/restricted detections
- show `Policy service unavailable` with a useful explanation

### Detector error
Do not silently send content if the classifier fails during a protected decision path.

### Unsupported attachment
Explain:
> AI Pass cannot inspect this file type locally yet. Your organisation's policy prevents sending uninspected files to this destination.

### UI injection failure
Do not break the host page. Log a non-sensitive technical error and avoid pretending protection is active.

---

# 11. Production architecture intentionally deferred

The following require a separate production design review:

- tenant isolation
- identity/SSO
- signed policy distribution
- production database
- encryption/key management
- regional hosting
- admin RBAC
- tamper protection
- Intune/Jamf deployment
- secure update path
- endpoint agents
- browser enterprise policy
- real AI vendor adapters
- audit evidence/retention
- SIEM/SOC integration
- regulatory mappings

The local demo should not pretend these exist.
