# AI Pass — Product Direction

## 1. Product thesis

AI Pass is not just a browser extension and not just a chat application.

The long-term product is a **company AI permission layer** that helps employees use AI safely while giving the organisation control over what data may be sent, where it may be sent, and under what conditions.

The product direction has two complementary surfaces:

1. **AI Pass Workspace** — a company-controlled AI workspace where approved models/services can eventually be offered behind company policy.
2. **AI Pass Guardrails** — browser/endpoint controls that protect employees when they use external AI tools directly.

The demo should prove the shared policy/security experience without overbuilding provider integrations.

---

# 2. AI Pass Workspace

`apps/demo-chat` is the technical demo package name for now. In the visible UI it should be presented as **AI Pass Workspace**.

The Workspace serves two purposes:

- a stable test surface for the current local demo; and
- an early product prototype of the future employee-facing AI workspace.

It should feel like a real product, not a fake ChatGPT clone.

## Current demo role

For the current demo, the Workspace may simulate destination/model context such as:

- Company AI
- External AI
- Unapproved AI

These states exist to exercise policy decisions.

Do **not** connect real model APIs unless a later phase explicitly adds them.

Do **not** turn the current demo into a full multi-model chat platform prematurely.

---

# 3. Future workspace direction

The future Workspace may allow employees to choose from company-approved AI providers/models, for example:

- OpenAI model
- Claude model
- Gemini model
- private/local model
- company-specific internal model

However, the important product abstraction is **approved destination**, not model-brand novelty.

The employee should think:

> “Which company-approved AI environment is appropriate for this work?”

The company should be able to define:

- approved providers
- approved account/workspace types
- permitted data classes
- regional/data-residency requirements
- whether redaction is required
- whether human approval is required
- whether the action must be blocked

The UI should therefore be designed so a future model/destination selector can exist naturally without requiring a redesign.

---

# 4. AI Pass Guardrails

The external guardrail layer is still a core part of the product.

It exists because employees will continue to use tools such as ChatGPT, Claude, Gemini, Perplexity, coding assistants, and future AI products outside the AI Pass Workspace.

The guardrail layer should eventually:

- detect supported external AI surfaces
- inspect outbound content locally
- understand destination/account context
- apply company policy
- allow, protect, or block
- provide the same intervention language as the Workspace
- report privacy-safe event metadata

The Workspace and Guardrails should share:

- policy model
- status language
- redaction logic
- admin rules
- visual identity
- audit/event schema

---

# 5. Product hierarchy

A useful mental model:

```text
AI Pass
│
├── Workspace
│   ├── company-controlled chat/work interface
│   ├── approved destinations/models
│   └── policy-aware routing/protection
│
├── Guardrails
│   ├── browser extension
│   ├── external AI interception
│   └── future endpoint coverage
│
└── Admin
    ├── policies
    ├── activity
    ├── devices
    └── approved destinations
```

This is one product, not three unrelated applications.

---

# 6. User experience principle

AI Pass should answer three simple questions for an employee:

1. **Can I use this AI?**
2. **Can I send this information?**
3. **If not, can AI Pass make it safe for me?**

The primary outcomes remain:

```text
Safe       -> send normally
Protected  -> transform/redact and continue
Blocked    -> stop and explain the safe alternative
```

The product must prefer safe enablement over punitive blocking.

---

# 7. What AI Pass is not

AI Pass is not:

- an employee surveillance product
- a generic chatbot wrapper
- a model marketplace
- an AI model vendor
- a SIEM replacement
- a general-purpose endpoint-security suite
- a prompt logging product
- a tool that stores employees' confidential prompts centrally

Avoid building features merely because other AI chat products have them.

---

# 8. Demo scope guardrail

The local demo should continue to focus on:

- policy decisions
- local detection
- redaction
- destination awareness
- admin visibility
- polished employee/admin experience

The following remain future work unless explicitly added to the roadmap:

- real OpenAI/Anthropic/Google API integration
- model billing
- model routing infrastructure
- conversation sync across providers
- production conversation history
- prompt libraries
- agents/tool calling
- enterprise knowledge retrieval

Do not let the Workspace concept expand the demo scope accidentally.

---

# 9. Naming

Preferred product names in the UI:

- **AI Pass** — product/company-facing security layer
- **AI Pass Workspace** — employee AI work surface
- **AI Pass Admin** — management console if a label is needed

Do not introduce placeholder brands such as “Orbit” unless the user explicitly approves a new name.

---

# 10. Design implication

The Workspace should look like a first-class product surface rather than a throwaway test harness.

However, its architecture must remain test-friendly and stable for browser-extension work.

Therefore:

- retain stable semantic DOM hooks
- keep simulated destination state deterministic
- keep provider integration abstract
- make the visible UI premium and original
- do not couple security logic to visual chat layout
