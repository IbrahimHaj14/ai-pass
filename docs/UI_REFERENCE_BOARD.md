# AI Pass — UI Reference Board

This document exists to remove ambiguity from future UI work.

It describes the visual characteristics AI Pass should move toward and the visual directions it should avoid.

Do not copy another product or template literally. Use references to understand material, spacing, hierarchy, motion, and product feel.

---

# 1. Target aesthetic

The target is:

> **Light glass minimalism for a premium AI/security workspace.**

Think:
- bright ambient canvas
- frosted white surfaces
- visible but controlled backdrop blur
- thin luminous borders
- soft dimensional shadows
- crisp dark typography
- one cool product accent
- semantic green/amber/red used sparingly
- elegant negative space
- high information clarity

The product should feel closer to premium fintech/productivity software than to a cybersecurity operations console.

---

# 2. User-provided references

The user supplied three strong aesthetic references.

## Reference A — glass finance dashboard

What to take from it:
- a large frosted translucent application panel
- layered floating navigation
- glass that visibly refracts/softens the background
- soft white borders
- depth created through blur + shadow + transparency
- warm ambient environment showing through the material
- rounded but controlled geometry

What **not** to copy:
- orange financial-brand palette
- banking card content
- literal photographic room background
- oversized concept-render effects that reduce application readability

For AI Pass, translate this into a bright abstract atmospheric canvas rather than a photo-real background.

## Reference B — light glass card over landscape

What to take from it:
- strong central frosted-white card
- material separation without a heavy border
- airy white space
- bright natural light
- simple dark typography
- subtle glass distortion/blur around card edges

What not to copy:
- literal landscape photography for the admin application
- decorative 3D flower/object

The useful principle is that glass feels richer when the background has subtle tonal information.

## Reference C — glassmorphism/neumorphism editorial panel

What to take from it:
- extremely light palette
- large frosted panel
- translucent sculptural objects
- pale blue/green atmospheric tint
- thin lines
- quiet modern typography
- almost weightless material feel

What not to copy:
- oversized editorial typography inside operational application screens
- floating decorative spheres throughout the product

---

# 3. Negative reference: current dark Orbit UI

The current Orbit-style Phase 1 implementation is explicitly **not** the target.

Problems:
- dark-first
- visually similar to many generic AI chat applications
- sidebar reads as a conventional dark slab
- glass material is not visually apparent
- surfaces rely more on dark color blocks than material/depth
- too little atmospheric light
- not distinctive enough

When revising the UI:
- preserve useful layout/functionality where appropriate
- replace the visual system substantially
- remove the Orbit brand
- do not merely invert the dark colors to white

A successful revision should look structurally and materially reconsidered, not like a theme toggle.

---

# 4. Researched references

## Apple — Materials / Liquid Glass

Reference:
https://developer.apple.com/design/human-interface-guidelines/materials

Useful principles:
- material should communicate hierarchy/function
- translucency must retain legibility
- glass works best on navigation, controls, overlays, and floating surfaces
- avoid making every content region equally translucent

AI Pass interpretation:
- glass navigation
- glass composer
- glass intervention cards
- glass key dashboard surfaces
- more opaque readable tables/forms inside the glass system

## Framer — Banks Classic

Reference:
https://www.framer.com/marketplace/templates/banks-classic/

Useful traits:
- light fintech aesthetic
- crisp minimal presentation
- modern bright visual hierarchy

Use as a directional reference for lightness and professionalism, not as a component/template source.

## Framer — Swiper

Reference:
https://www.framer.com/marketplace/templates/swiper/

Useful traits:
- real product UI rather than only marketing art
- coherent light/dark design system
- polished fintech information architecture
- responsive reusable components

Use as reference for dashboard/product composition.

## Framer — Saro

Reference:
https://www.framer.com/marketplace/templates/saro/

Useful traits:
- calm premium gray system
- one bright accent
- clean motion
- modern product UI

Use as reference for restraint.

## Framer — PingPong App

Reference:
https://www.framer.com/marketplace/templates/pingpong-app/

Useful traits:
- modern fintech/SaaS framing
- English/Arabic and RTL awareness
- relevant future consideration for GCC/MENA customers

Do not add Arabic/RTL implementation merely for the UI redesign unless explicitly requested; retain this as a future design consideration.

## Vercel Design

Reference:
https://vercel.com/design

Useful principles:
- disciplined hierarchy
- consistent design system
- careful interaction details
- minimal visual noise

---

# 5. Material hierarchy

The desired screen should visibly contain different material levels.

### Background
Soft, atmospheric, off-white.

### Navigation shell
Frosted glass, clearly floating.

### Key cards
Frosted or semi-frosted glass.

### Dense data
More opaque white surface with minimal translucency.

### Popovers/interventions
Strong glass with deeper blur/shadow.

### Accent action
Small controlled area of indigo/aqua/semantic color.

If every card has the same transparency and blur, the design has failed to establish hierarchy.

---

# 6. Desired admin impression

On first view, the admin should communicate:

> “This is a calm, premium AI control layer.”

Not:

> “This is a security dashboard full of alerts.”

The screen should be visually simple enough to understand quickly.

Primary information:
- protection status
- protected devices
- safe/protected/blocked activity
- recent events
- policy health

Avoid filling whitespace with low-value analytics.

---

# 7. Desired Workspace impression

On first view, AI Pass Workspace should communicate:

> “This is the approved place to work with AI inside my company.”

The composer should be the most polished object.

The protection state should feel native:

```text
Protected by AI Pass
```

not like a third-party banner bolted onto the page.

The future destination/model control should feel like a small contextual selector, not a marketplace.

---

# 8. Desired intervention impression

The intervention should feel like intelligent assistance.

### Safe
Almost invisible.

### Protected
The strongest signature interaction:
- beautiful glass card
- calm amber cue
- clear reason
- `Send safely` as the obvious next step

### Blocked
Firm but calm:
- light glass
- restrained red cue
- explain why
- offer safe next action where possible

Never use alarmist security aesthetics.

---

# 9. Originality rule

AI Pass must be an original product design.

Do not:
- reproduce another template's exact layout
- use another product's proprietary illustrations/assets
- copy branded icons/graphics
- duplicate an Apple/Framer design pixel-for-pixel

It is acceptable to apply general patterns such as:
- frosted material
- floating rails
- atmospheric gradients
- compact cards
- glass popovers
- restrained motion

---

# 10. Quick visual test

Before accepting a redesigned screen, ask:

1. Does it look light immediately?
2. Is the glass material clearly visible without making text harder to read?
3. Does it have more depth than a flat white SaaS dashboard?
4. Is it calmer than a cybersecurity dashboard?
5. Does it look original rather than like ChatGPT/Claude?
6. Would the UI still look premium if all demo data were replaced with real enterprise data?
7. Does the `Protected` state feel like a signature product experience?

If any answer is no, continue refining.
