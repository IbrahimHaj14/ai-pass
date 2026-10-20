# AI Pass — Design System

## 1. Design direction: **Signal Glass**

AI Pass should look like a premium, current enterprise product that happens to be security software — not like a traditional SIEM dashboard and not like a generic template.

The visual direction is called **Signal Glass**:

- graphite/charcoal foundation rather than pure black
- high-legibility near-white typography
- restrained indigo/periwinkle primary accent
- optional teal secondary accent
- semantic green / amber / red for AI Pass decisions
- semi-transparent glass for navigation, floating controls and intervention layers
- more solid surfaces for dense content
- soft hairline borders
- subtle ambient light/gradient in the page background
- crisp iconography
- purposeful micro-motion
- generous spacing without wasting screen area

The result should feel:
- calm
- intelligent
- fast
- trustworthy
- slightly futuristic
- enterprise-ready

It should **not** feel:
- hacker/cyberpunk
- neon
- gaming-oriented
- sterile all-white
- overloaded with charts
- like a default shadcn dashboard
- like a direct copy of Linear, Vercel, Apple, Attio, or the existing Chromastic site

---

# 2. Research-informed principles

## Existing Chromastic reference

The user's `ai-infrastructure-website` already uses:
- Geist
- rounded geometry
- a cool blue accent
- soft neutral surfaces
- pill controls
- sticky/translucent navigation
- soft shadows
- clean hierarchy

Keep the **discipline and simplicity**, but move AI Pass away from its overly white/flat presentation.

Do not copy the marketing-site scale or huge hero typography into the application.

## Linear
Current Linear design guidance emphasizes:
- calmer interface
- consistent header/navigation placement
- navigation receding so work content remains primary
- dense information without equal visual weight everywhere

Apply:
- dimmer sidebar than workspace
- predictable page header
- quiet secondary controls
- strong active-content focus

## Attio
Useful patterns:
- compact high-speed UI
- keyboard-first navigation
- command palette/search
- data-rich lists that still look polished
- modern elevated dark surfaces

Apply:
- compact admin rows
- direct controls
- optional command/search affordance later
- avoid oversized dashboard cards

## Apple material / Liquid Glass
Apple's material guidance treats glass as a **functional layer**, not decoration everywhere.

Apply:
- glass in sidebar/top chrome, floating AI Pass intervention, transient overlays/popovers
- solid or lightly translucent surfaces for dense tables/content
- preserve contrast
- do not stack multiple blurry glass cards without reason

## Vercel interface guidance
Apply:
- design empty, loading, dense and error states
- avoid dead ends
- inline help before tooltips
- use status text/icons in addition to color
- error messages explain the next action
- stable loading skeletons

## Accessibility foundation
Prefer Radix primitives or equivalent accessible primitives for:
- dialog
- popover
- dropdown
- tabs
- tooltip
- switches
- keyboard/focus management

---

# 3. Theme

## Default theme

For the demo, use a **dark-first graphite theme**.

Light mode can be added later; do not spend initial phases duplicating every surface.

### Core tokens

These are starting points, not immutable branding:

```css
--bg-0: #090C11;
--bg-1: #0D1118;
--surface-1: #111722;
--surface-2: #151D29;
--surface-3: #1A2331;

--glass: rgba(20, 27, 39, 0.72);
--glass-strong: rgba(18, 24, 34, 0.88);

--line: rgba(255, 255, 255, 0.08);
--line-strong: rgba(255, 255, 255, 0.14);

--text-1: #F4F7FB;
--text-2: #A8B1C0;
--text-3: #727D8E;

--accent: #7C8DFF;
--accent-strong: #91A0FF;
--accent-soft: rgba(124, 141, 255, 0.14);

--teal: #59D5C3;
--teal-soft: rgba(89, 213, 195, 0.13);

--green: #50D890;
--green-soft: rgba(80, 216, 144, 0.13);

--amber: #F0B85A;
--amber-soft: rgba(240, 184, 90, 0.14);

--red: #FF737D;
--red-soft: rgba(255, 115, 125, 0.14);
```

Do not turn every surface into a colored gradient.

Accent color should occupy a small portion of the screen.

---

# 4. Background treatment

Admin and demo chat can use a subtle ambient background:

```text
deep graphite base
+
very soft radial indigo glow in one corner
+
very soft teal/blue secondary glow
+
optional fine noise texture
```

Requirements:
- the effect is almost imperceptible
- no animated rainbow gradients
- no bright blobs competing with data
- content remains readable with glows removed

---

# 5. Material system

Use three levels.

## Level 0 — canvas
Opaque dark background.

## Level 1 — content
Mostly solid or 92–98% opaque surfaces.

Used for:
- tables
- activity lists
- policy forms
- dense information
- settings

## Level 2 — glass / transient chrome
Blurred/translucent.

Used for:
- sidebar
- top navigation/context bar
- dropdowns/popovers
- floating AI Pass intervention card
- command/search surface
- modal chrome

Suggested CSS direction:

```css
background: rgba(...);
backdrop-filter: blur(18px) saturate(130%);
border: 1px solid rgba(255,255,255,.09);
box-shadow:
  0 18px 50px rgba(0,0,0,.28),
  inset 0 1px 0 rgba(255,255,255,.05);
```

Avoid glass on glass on glass.

---

# 6. Geometry

Preferred:
- application panels: 16–20 px radius
- large floating intervention: 18–22 px
- buttons: 10–14 px radius
- pills/status chips: fully rounded
- table rows: 10–12 px hover region if individually surfaced

Do not use giant 40–60 px marketing-site radii throughout the admin application.

---

# 7. Typography

Default: **Geist**.

Hierarchy:

```text
Page title       28–34 px / medium
Section title    17–20 px / medium
Body             14–15 px
Compact body     13 px
Metadata         12–13 px
Metric value     26–36 px / medium
```

Principles:
- tight but not extreme negative tracking
- use tabular numerals for metrics
- avoid giant headings inside the product
- sentence case labels
- no ALL CAPS except tiny optional technical tags

---

# 8. Navigation

## Admin shell

Desktop:
- left sidebar about 232–252 px expanded
- optional compact state later
- sidebar slightly dimmer than main workspace
- top context row belongs to content, not a second oversized nav bar

Suggested navigation:
- Overview
- Activity
- Policies
- Devices
- Settings

Bottom:
- demo tenant
- user/avatar
- version/environment indicator

Active nav:
- subtle accent-tinted surface
- icon
- text
- possibly a thin luminous edge, not a huge filled block

Do not put all navigation inside floating pill buttons.

---

# 9. Dashboard composition

Avoid:
- 8–12 equal KPI cards
- donut charts for everything
- large empty cards
- “security operations center” visual clutter

Prefer:

```text
[ Page title + concise context + protected-state indicator ]

[ Protected devices ] [ Protected interactions ] [ Blocked ]

[ Recent activity — primary wide surface         ]
[                                              ]
[                                              ]

[ AI destinations / policy health ] [ Devices ]
```

The activity surface should be the visual anchor.

Metrics should be compact and calm.

---

# 10. AI Pass intervention UI

This is the most important visual component in the product.

It appears close to the AI composer, not in the center of the page unless required.

## Shared anatomy

```text
[ shield/status icon ]  AI Pass        [ status pill ]

Title
One-line explanation

Detected categories (if useful)
- category label
- never raw value

[ primary action ] [ secondary action ]
```

### SCANNING
Use:
- compact progress/sweep animation
- label `Checking before send…`
- duration should reflect real work; do not artificially delay normal sends

### GREEN
Do not interrupt with a large modal.

Use:
- small confirmation pill/card
- green icon
- `Safe to send`
- auto-dismiss quickly

### YELLOW
This is the signature state.

Use:
- warm amber edge/light
- clear title such as `Sensitive information detected`
- short explanation
- category chips
- primary: `Send safely`
- secondary: `Cancel`
- optional small `Why?`

The action should feel constructive, not punitive.

### RED
Use:
- restrained red, not flashing
- `Restricted information`
- reason tied to company policy
- explain the safe next action
- no scary “BREACH DETECTED” copy

The card should feel authoritative and calm.

---

# 11. Status language

Never rely on color alone.

Use this consistent trio:

```text
GREEN   Safe
YELLOW  Protected / Needs protection
RED     Blocked
```

In tables:

```text
✓ Safe
◇ Protected
! Blocked
```

Exact icons may vary, but use one icon system consistently.

---

# 12. AI Pass protected indicator

Near the demo-chat composer, show a small unobtrusive state:

```text
shield icon  Protected by AI Pass
```

Interaction:
- hover/click can show current policy/service
- briefly animates when scanning
- should not constantly pulse

This helps users trust that protection is active without occupying attention.

---

# 13. Motion

Motion should communicate causality.

Preferred timings:
- hover/focus: 120–180 ms
- small open/close: 160–220 ms
- intervention enter/exit: 180–260 ms
- page section transitions: <= 300 ms

Use spring motion only for:
- intervention expansion
- compact status transitions
- drawer/popover arrival

Avoid:
- looping ambient object animations
- parallax
- scroll hijacking
- bouncing icons
- continuous pulsing
- animated backgrounds

### Reduced motion
Respect:

```css
@media (prefers-reduced-motion: reduce)
```

Remove transforms/springs and keep instant or simple opacity changes.

---

# 14. Microinteractions

Good:
- active nav background glides subtly
- status pill cross-fades between Scanning/Safe/Protected/Blocked
- buttons depress 1 px on pointer down
- table row gets a subtle border/surface lift on hover
- policy save shows compact confirmation
- new activity row gently appears
- `Send safely` visually transforms the outgoing message state

Bad:
- confetti
- sound effects
- excessive glow
- animated gradients
- shaking blocked content
- “hacker scan” effects

---

# 15. Tables and activity lists

Admin activity should look closer to a premium productivity app than a SIEM.

Columns:
- time
- user/device
- destination
- decision
- categories
- action

Use:
- quiet separators
- compact 44–52 px rows
- hover detail
- readable badges
- sticky header if needed
- tabular numeric/time styling

Do not show raw prompt previews.

---

# 16. Policy UI

Policies should be intuitive, not written like firewall ACLs.

Prefer sentence-like controls:

```text
When data is [Confidential]
and destination is [External AI]
then [Block]
```

or a simple matrix with strong defaults.

Custom company term editor:

```text
Project Falcon     Restricted     [edit]
Client Atlas       Confidential   [edit]

+ Add protected term
```

Explain consequences inline.

---

# 17. Empty states

Every major screen needs a real empty state.

Examples:

### No activity
> No AI activity yet  
> Use the local demo chat with AI Pass enabled to generate your first event.

### No custom terms
> No company-specific terms yet  
> Add project, client, or document names that should receive special protection.

### Device offline
> This device has not checked in recently.  
> Confirm the extension is enabled and the local API is reachable.

No decorative giant illustrations are required.

---

# 18. Loading states

Skeletons must match the final layout closely to avoid content jumps.

Use subtle shimmer only if restrained.

Do not show full-page spinners for local data that can load incrementally.

---

# 19. Error states

Errors should contain a recovery action.

Bad:
> Error 500

Good:
> Couldn’t load policies.  
> The local AI Pass API is not responding.  
> `Retry`

Do not reveal sensitive values in errors.

---

# 20. Icons

Use one restrained outline icon system.

Recommended style:
- 1.5–2 px strokes
- rounded geometry
- simple metaphors
- no multicolored icon pack

Potential icons:
- shield
- activity
- policy/sliders
- laptop/device
- lock
- check
- triangle-alert
- eye/visibility where appropriate

---

# 21. Accessibility

Minimum:
- WCAG AA contrast target
- visible focus ring
- full keyboard navigation for app controls
- escape closes modal/popover
- ARIA names for icon-only controls
- color + icon + text for states
- reduced motion
- correct heading hierarchy
- forms have labels
- tooltips are not the only place critical information appears

Use accessible primitives rather than rebuilding focus management.

---

# 22. Responsive behavior

Primary target: desktop enterprise application.

Test:
- 1600 px
- 1440 px
- 1280 px
- 1024 px

At narrower widths:
- sidebar may collapse
- metrics can wrap
- activity can reduce secondary columns
- do not turn the dashboard into horizontal overflow

Extension intervention should remain usable in narrow viewport/composer contexts.

---

# 23. Design acceptance checklist

Before any UI-heavy phase passes:

- [ ] Looks intentionally designed, not scaffolded
- [ ] Main work area dominates over navigation
- [ ] Accent usage is restrained
- [ ] Glass appears only where it helps hierarchy
- [ ] Dense content uses readable solid surfaces
- [ ] Green/yellow/red states are obvious without color alone
- [ ] No raw sensitive content appears in admin UI
- [ ] Hover/focus/pressed/disabled states exist
- [ ] Empty/loading/error states exist where relevant
- [ ] Reduced motion works
- [ ] 1024–1600 px widths checked
- [ ] No clipped text or unintended scrollbars
- [ ] No giant marketing typography inside app
- [ ] No generic template/dashboard feel

---

# 24. Design references researched

These are references for principles, not templates to copy.

- Existing Chromastic repository: clean Geist typography, cool accent, soft surfaces, rounded controls.
- Linear 2026 UI refresh: calm hierarchy, consistent navigation, dimmer sidebars so content stays primary.
- Attio: high-speed compact UI, command/search orientation, polished data surfaces.
- Apple Human Interface Guidelines — Materials / Liquid Glass: glass as a functional navigation/control layer, use sparingly.
- Vercel Web Interface Guidelines: all UI states, recovery paths, redundant status cues, inline help.
- Radix Primitives: accessible low-level interaction behavior.
- shadcn/ui: open component code may be used as a structural starting point, but final visuals must be AI Pass-specific.

Useful URLs:
- https://linear.app/now/behind-the-latest-design-refresh
- https://linear.app/changelog/2026-03-12-ui-refresh
- https://attio.com/
- https://developer.apple.com/design/human-interface-guidelines/materials
- https://vercel.com/design/guidelines
- https://www.radix-ui.com/primitives
- https://ui.shadcn.com/

---

# 25. One-line visual brief for Codex

> Build AI Pass like a calm premium productivity/security application: graphite workspace, restrained indigo/teal signal color, solid readable data surfaces, selective glass navigation and interventions, crisp Geist typography, compact information density, and purposeful motion — futuristic enough to feel new, disciplined enough to trust with enterprise security.
