# AI Pass — Design System

## 1. Visual direction: **Light Glass Minimalism**

This document is the visual source of truth for AI Pass.

The previous dark/graphite direction is retired. **Do not use it as a fallback.**

AI Pass must be a **light-first, premium glassmorphism product**: airy, minimal, softly dimensional, modern, calm, and clearly usable as enterprise software.

The intended impression is:

> Apple-like material discipline + premium fintech clarity + modern SaaS interaction design, expressed as an original AI Pass interface.

It should feel:
- bright
- calm
- premium
- intelligent
- lightweight
- trustworthy
- slightly futuristic
- highly polished

It must **not** feel:
- like the current dark “Orbit” chat shell
- like a generic ChatGPT clone
- like a generic shadcn dashboard
- like a dark security operations console
- cyberpunk, neon, hacker-themed, or gaming-oriented
- flat white-on-white with no depth
- over-blurred or illegible
- like a concept render that sacrifices usability for effects

---

# 2. Non-negotiable visual rules

1. **Light mode is the default and only required theme for the demo.**
2. The canvas must not be pure white. Use a soft atmospheric backdrop so glass is visually meaningful.
3. Glassmorphism must be clearly visible in navigation, floating controls, key cards, composer/intervention surfaces, and selected dashboard modules.
4. Dense tables and forms may use more opaque translucent surfaces for readability.
5. Use blur, transparency, edge highlights, and soft shadow together. Transparency alone is not glassmorphism.
6. Keep color restrained. The interface is primarily neutral with one cool product accent plus semantic state colors.
7. Motion must be subtle and functional.
8. The UI must remain accessible and legible at all times.
9. No dark-first fallback. Do not restore the previous graphite palette.
10. Do not invent a separate product brand such as “Orbit.” The product is **AI Pass**; the controlled chat surface is **AI Pass Workspace**.

---

# 3. Reference intent

See `docs/UI_REFERENCE_BOARD.md` for the explicit visual reference hierarchy.

The key visual traits to reproduce **in spirit, not by copying** are:
- translucent white surfaces over a softly varied background
- visible backdrop blur
- fine bright borders / edge highlights
- deep but diffuse shadows
- floating navigation and panels
- comfortable negative space
- minimal typography
- one restrained accent
- premium product/fintech polish

The negative reference is the existing dark “Orbit” implementation: technically clean, but too dark, flat, conventional, and generic for the intended product identity.

---

# 4. Canvas and atmosphere

True glass needs visual information behind it. Therefore the page background should be a quiet atmospheric canvas rather than plain `#fff`.

## Preferred canvas

Use a warm/cool off-white foundation with very soft radial or mesh gradients:

```css
--canvas: #F5F7FB;
--canvas-warm: #FBFAF7;
--canvas-blue: #EEF4FF;
--canvas-mint: #EFFBF7;
```

Suggested background concept:

```css
background:
  radial-gradient(circle at 18% 8%, rgba(126, 151, 255, .14), transparent 34%),
  radial-gradient(circle at 84% 18%, rgba(116, 218, 198, .12), transparent 30%),
  radial-gradient(circle at 50% 100%, rgba(255, 215, 177, .10), transparent 34%),
  #F5F7FB;
```

Optional:
- extremely subtle fine noise/grain at <= 2–3% perceived opacity
- static blurred ambient shapes behind selected areas

Do not:
- animate the background continuously
- use rainbow gradients
- use saturated blobs
- place busy photography behind the admin application

The background should make the glass readable while still disappearing from conscious attention.

---

# 5. Color tokens

These are the preferred starting tokens. Minor tuning is allowed only if contrast/material quality improves.

```css
:root {
  /* canvas */
  --bg-canvas: #F5F7FB;
  --bg-canvas-warm: #FBFAF7;

  /* glass / surfaces */
  --glass-subtle: rgba(255, 255, 255, 0.42);
  --glass: rgba(255, 255, 255, 0.58);
  --glass-strong: rgba(255, 255, 255, 0.76);
  --surface-solid: rgba(255, 255, 255, 0.90);

  /* borders/highlights */
  --glass-line: rgba(255, 255, 255, 0.72);
  --glass-line-strong: rgba(255, 255, 255, 0.92);
  --line-neutral: rgba(50, 66, 92, 0.10);
  --line-neutral-strong: rgba(50, 66, 92, 0.16);

  /* text */
  --text-1: #152033;
  --text-2: #556176;
  --text-3: #7D889A;
  --text-inverse: #FFFFFF;

  /* product accent */
  --accent: #6979F8;
  --accent-strong: #5568F2;
  --accent-soft: rgba(105, 121, 248, 0.12);
  --accent-softer: rgba(105, 121, 248, 0.07);

  /* optional secondary signal */
  --aqua: #43C8B5;
  --aqua-soft: rgba(67, 200, 181, 0.11);

  /* semantic states */
  --safe: #249C68;
  --safe-soft: rgba(36, 156, 104, 0.10);
  --protect: #C98724;
  --protect-soft: rgba(201, 135, 36, 0.11);
  --block: #D5535D;
  --block-soft: rgba(213, 83, 93, 0.10);
}
```

Accent color should occupy a small minority of the interface.

Never tint the entire dashboard blue/purple.

---

# 6. The glass material recipe

Glass should look physically plausible enough to feel premium but not photorealistic or distracting.

## Standard glass

```css
.glass {
  background: rgba(255, 255, 255, 0.58);
  border: 1px solid rgba(255, 255, 255, 0.72);
  backdrop-filter: blur(22px) saturate(135%);
  -webkit-backdrop-filter: blur(22px) saturate(135%);
  box-shadow:
    0 22px 60px rgba(40, 56, 82, 0.10),
    0 4px 16px rgba(40, 56, 82, 0.05),
    inset 0 1px 0 rgba(255, 255, 255, 0.88);
}
```

## Strong glass
For composers, modals, intervention cards, and panels requiring stronger readability:

```css
.glass-strong {
  background: rgba(255, 255, 255, 0.76);
  border: 1px solid rgba(255, 255, 255, 0.90);
  backdrop-filter: blur(28px) saturate(130%);
  -webkit-backdrop-filter: blur(28px) saturate(130%);
  box-shadow:
    0 28px 70px rgba(35, 53, 83, 0.13),
    inset 0 1px 0 rgba(255, 255, 255, 0.92);
}
```

## Dense information surface
Tables/forms can be clearer:

```css
.surface-readable {
  background: rgba(255, 255, 255, 0.86);
  border: 1px solid rgba(61, 75, 98, 0.08);
  box-shadow: 0 14px 36px rgba(45, 60, 88, 0.07);
}
```

## Important
A plain translucent white card with no backdrop blur, edge highlight, or shadow is **not sufficient**.

Do not stack many equally transparent glass cards on top of each other. Use a hierarchy of canvas → glass shell → readable surface.

---

# 7. Geometry

The interface should feel softly sculpted, not bubbly.

Recommended:
- floating app shell/sidebar: 22–28 px radius
- major cards/panels: 18–24 px
- composer/intervention: 20–24 px
- buttons/inputs: 11–14 px
- compact controls: 9–12 px
- pills/badges: fully rounded

Avoid:
- 40–60 px radii on every application component
- completely square enterprise panels
- excessive pill-shaped controls

---

# 8. Typography

Default font: **Geist**.

Use a minimal typography scale:

```text
Page title        30–34 px / 540–600
Large metric      28–36 px / 560–620
Section title     17–20 px / 550–620
Body              14–15 px / 400–480
Compact body      13–14 px
Metadata          12–13 px
```

Rules:
- sentence case
- tabular numerals for metrics/time
- restrained negative letter spacing only on headings
- no enormous marketing typography inside the application
- no monospaced “security terminal” visual language except actual code/IDs

---

# 9. Overall application composition

The app should feel like a set of lightly floating glass layers inside a bright atmospheric workspace.

## Admin

Preferred desktop composition:

```text
┌───────────────────────────────────────────────────────────────┐
│  floating glass sidebar  │  workspace header                │
│                          │                                  │
│  Overview                │  title / context / actions       │
│  Activity                │                                  │
│  Policies                │  metrics / activity / devices    │
│  Devices                 │                                  │
│  Settings                │                                  │
└───────────────────────────────────────────────────────────────┘
```

The sidebar should be a **light translucent floating rail/panel**, not a dark slab touching every viewport edge.

Suggested spacing:
- 16–24 px outer canvas inset
- 18–24 px gap between floating sidebar and main content
- main content max width appropriate to viewport, not a narrow centered marketing column

## Navigation material
- stronger glass than the page canvas
- subtle edge highlight
- soft shadow
- active item uses a quiet accent-tinted inset surface
- icons are outline, monochrome, restrained

---

# 10. Admin dashboard

The admin dashboard must look like a premium management workspace, not a SOC dashboard.

Primary hierarchy:

```text
[ Overview                                        status ]
[ concise subtitle                         primary action ]

[ Protected devices ] [ Protected interactions ] [ Blocked ]

[ Recent activity — wide anchor surface                         ]
[                                                               ]

[ AI destinations / policy health ] [ Device protection        ]
```

## Metrics
- compact
- strong number + quiet label
- optional tiny trend/secondary value
- no giant colorful KPI cards

## Activity
- should be the main information anchor
- readable translucent/solid surface inside the glass workspace
- 44–52 px rows
- quiet dividers
- status badge + text
- no raw prompt excerpts

## Charts
Use only when they genuinely improve understanding.
Do not add charts merely to fill the dashboard.

If a chart exists:
- thin lines
- soft grid
- restrained accent
- no rainbow legend

---

# 11. AI Pass Workspace (formerly generic demo chat)

`apps/demo-chat` remains the technical package name for now, but the visible product surface should be branded **AI Pass Workspace**.

It is both:
1. the stable local test surface for the demo; and
2. a visual prototype of the future company-controlled AI workspace described in `docs/PRODUCT_DIRECTION.md`.

It must not visually imitate ChatGPT or Claude closely.

## Workspace layout

Preferred:
- airy canvas
- light floating/glass navigation
- central conversation area with generous whitespace
- small context/model/destination control near the top
- glass composer floating near the bottom
- AI Pass protection state integrated into the composer

### Important scope rule
Do **not** integrate real model providers during the UI revision.
A model/destination selector may be shown only if it represents existing demo destination state or a clearly non-functional future-ready shell. Do not invent provider integrations.

## Conversation styling
- user messages: lightly tinted/opaque bubble or aligned card
- assistant messages: mostly open text with subtle structural grouping
- avoid giant dark chat bubbles
- avoid dense permanent left conversation rail unless it improves demo usability
- if recent chats are shown, they should be light and quiet

## Composer
The composer is a signature object:
- strong frosted glass
- subtle floating shadow
- attachment/control buttons inset cleanly
- send button uses product accent
- `Protected by AI Pass` status is integrated, not an afterthought

---

# 12. AI Pass intervention UI

This is the most important component in the product.

It should feel like a small piece of intelligent glass appearing at the exact moment help is needed.

Shared anatomy:

```text
[ shield/status icon ] AI Pass               [ status ]

Title
Short explanation

[ optional category chips ]

[ primary action ]  [ secondary ]
```

## Scanning
- small progress/sweep state
- copy: `Checking before send…`
- no artificial delay
- no “cyber scan” animation

## Safe / Green
- minimal acknowledgement
- green icon + `Safe to send`
- small pill/card
- auto-dismiss quickly

## Protect / Yellow
This is the signature interaction.

Use:
- frosted white panel
- subtle amber semantic ring/glow, not full amber fill
- title: `Sensitive information detected`
- category chips without raw values
- primary: `Send safely`
- secondary: `Cancel`
- optional `Why?`

The interaction should feel helpful and productive.

## Block / Red
Use:
- frosted white panel
- restrained red edge/ring
- calm copy such as `Restricted information`
- explain the company-policy reason
- provide safe next action
- no flashing, shaking, alarm styling, or “BREACH” copy

---

# 13. Status language

Never rely on color alone.

Canonical states:

```text
Safe       green   check/shield icon
Protected  amber   shield/spark icon
Blocked    red     alert/lock icon
```

Possible compact table treatment:

```text
✓ Safe
◇ Protected
! Blocked
```

---

# 14. Controls and inputs

Inputs should feel lightly inset into the glass system.

Use:
- translucent/near-white fill
- 1 px neutral border
- subtle inner highlight
- clear focus ring using the accent
- 42–48 px target height for primary form controls

Buttons:
- primary: accent fill or high-opacity accent gradient only if extremely restrained
- secondary: glass/neutral
- tertiary: text/icon

No glossy 2000s-style button effects.

---

# 15. Motion

Motion communicates state and material.

Timing:
- hover/focus: 120–180 ms
- popover/menu: 160–220 ms
- intervention enter/exit: 180–260 ms
- panel transition: <= 300 ms

Allowed:
- 1–2 px lift on hover
- soft shadow interpolation
- fade + slight scale for intervention cards
- active nav indicator glide
- status crossfade
- subtle composer focus expansion

Avoid:
- continuous floating objects
- looping glow
- animated mesh background
- bouncing
- parallax
- scroll hijacking
- blur animation that makes text unreadable

Respect `prefers-reduced-motion`.

---

# 16. Microinteractions

Desired:
- glass panel edge brightens slightly on hover/focus
- buttons depress 1 px on press
- new activity rows fade/slide in subtly
- `Send safely` visibly transitions the outgoing message into protected state
- policy save gives a compact confirmation
- model/destination control opens as a small floating glass popover

Do not add decorative effects with no state meaning.

---

# 17. Empty/loading/error states

Design all states, not only populated screens.

## Empty
Example:
> No AI activity yet  
> Use AI Pass Workspace to generate your first protected event.

## Loading
Use layout-matching skeletons.
Avoid full-screen spinners.

## Error
Every error has a recovery path.

Example:
> Couldn’t load policies.  
> The local AI Pass API is not responding.  
> `Retry`

Errors must never display sensitive raw values.

---

# 18. Icons

Use one restrained outline icon system such as Lucide.

Style:
- 1.5–2 px stroke
- simple geometry
- monochrome by default
- semantic color only when state matters

Do not use mixed icon families or colorful illustrations as navigation icons.

---

# 19. Accessibility

Minimum:
- WCAG AA contrast target
- visible keyboard focus
- keyboard-accessible controls
- Escape closes overlays
- ARIA names for icon-only actions
- status conveyed by icon + text + color
- reduced-motion support
- correct heading hierarchy
- labels for all form controls
- tooltips are supplementary, not the only explanation

Glass surfaces must remain readable even if backdrop filtering is unavailable.
Provide a solid/translucent fallback.

---

# 20. Responsive targets

Primary application target is desktop.

Verify:
- 1600 px
- 1440 px
- 1280 px
- 1024 px

At narrower widths:
- floating sidebar may collapse to an icon rail/drawer
- metric cards wrap
- secondary activity columns may hide
- glass panels must not create horizontal overflow

The intervention component must remain usable in narrow browser/composer contexts.

---

# 21. Template and product references

These references are for **design principles and layout ideas only**. Do not copy licensed templates or proprietary visual assets.

### Framer — Banks Classic
Use as reference for:
- bright light-theme fintech presentation
- crisp typography
- clean modern product framing

https://www.framer.com/marketplace/templates/banks-classic/

### Framer — Swiper
Use as reference for:
- polished real product UI
- SaaS/fintech information structure
- theme/system consistency

https://www.framer.com/marketplace/templates/swiper/

### Framer — Saro
Use as reference for:
- calm premium gray system
- restrained single-accent approach
- clean animated product presentation

https://www.framer.com/marketplace/templates/saro/

### Framer — PingPong App
Use as reference for:
- MENA-aware SaaS presentation
- bilingual/RTL thinking for future expansion
- modern fintech framing

https://www.framer.com/marketplace/templates/pingpong-app/

### Apple Human Interface Guidelines — Materials
Use as reference for material hierarchy and legibility, not as a component library.

https://developer.apple.com/design/human-interface-guidelines/materials

### Vercel Design / Web Interface Guidelines
Use for interaction/state discipline and interface craft.

https://vercel.com/design

### Existing Chromastic site
Keep its disciplined typography, spacing, and clean simplicity, but AI Pass must be more dimensional and glass-led rather than flat/white.

---

# 22. Design acceptance checklist

A UI revision does not pass unless all of the following are true:

- [ ] Default theme is visibly light, not dark/graphite
- [ ] The old “Orbit” brand/dark shell is gone
- [ ] Visible glassmorphism exists in navigation and key interaction surfaces
- [ ] Glass uses blur + transparency + edge highlight + soft shadow
- [ ] Background has enough subtle tonal variation for glass to read
- [ ] The interface is still highly legible
- [ ] Admin looks like a premium management workspace, not a SOC console
- [ ] AI Pass Workspace does not look like a generic ChatGPT clone
- [ ] Composer is a polished signature glass surface
- [ ] Green/yellow/red states use icon + label + semantic accent
- [ ] Yellow `Send safely` state receives special visual care
- [ ] No raw sensitive content appears in admin UI
- [ ] Hover/focus/pressed/disabled states exist
- [ ] Empty/loading/error states are designed
- [ ] Reduced-motion behavior works
- [ ] 1024, 1280, 1440, and 1600 px widths are checked
- [ ] No clipping or unintended horizontal scrolling
- [ ] No giant marketing typography inside the application
- [ ] No unnecessary chart/dashboard filler
- [ ] UI looks intentionally designed rather than scaffolded

---

# 23. One-line visual brief for Codex

> Build AI Pass as a bright premium glassmorphism workspace: airy off-white atmospheric canvas, frosted white floating surfaces, visible blur and edge highlights, crisp dark typography, restrained indigo/aqua accents, soft dimensional shadows, elegant spacing, and purposeful motion — modern enough to feel state-of-the-art, disciplined enough to trust with enterprise security.
