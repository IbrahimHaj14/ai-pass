# AI Pass — Post-Phase-2 UI Revision Spec

## Purpose

This document defines the one-time visual revision to perform **after Phase 2 is complete and accepted**.

It exists because the first Phase 1 implementation used a dark “Orbit”-style direction that is no longer desired.

This revision is **not a new product-functionality phase**. It is a focused visual/product-shell correction before extension interception work continues.

Source-of-truth documents:
- `docs/DESIGN_SYSTEM.md`
- `docs/UI_REFERENCE_BOARD.md`
- `docs/PRODUCT_DIRECTION.md`
- `docs/ARCHITECTURE.md`

---

# 1. Preserve

Preserve working Phase 0–2 engineering unless a small refactor is necessary for the visual system.

Preserve:
- monorepo architecture
- shared UI package concept
- accessibility behavior
- component APIs where sensible
- policy-engine code/tests
- existing test/tool configuration
- semantic DOM hooks used or planned for the demo chat
- responsive behavior that is already correct

Do not rewrite unrelated security/policy code for aesthetic reasons.

---

# 2. Remove / replace

The revision must remove or substantially replace:

- dark-first graphite theme
- “Orbit” visible branding
- dark full-height slab sidebar
- generic ChatGPT-like dark chat appearance
- flat dark cards
- dark composer
- dark background as the primary application canvas
- styling that only changes colors but keeps the same dark-console material logic

Do not implement a simple light-mode inversion. Reconsider material, spacing, depth, and composition.

---

# 3. New visible product naming

Use:

```text
AI Pass
AI Pass Workspace
```

Do not use:

```text
Orbit
```

The admin may simply use the AI Pass wordmark rather than “AI Pass Admin” unless a page specifically needs that label.

---

# 4. Required design direction

Implement **Light Glass Minimalism**.

Required traits:
- light atmospheric off-white canvas
- visible frosted-glass material
- white translucent surfaces
- real backdrop blur
- fine bright edge highlights
- diffuse soft shadows
- crisp dark navy/graphite text
- restrained indigo primary accent
- optional aqua secondary signal
- semantic green / amber / red only for status
- spacious but efficient layout
- subtle purposeful motion

Glass must be visually obvious without harming readability.

---

# 5. Required surfaces to revise

## A. Admin shell

Revise:
- application canvas
- navigation/sidebar
- top/header context
- metric cards
- recent activity surface
- policy/device preview surfaces
- buttons/badges/inputs
- empty/loading/error shells if present

Target:
> premium light management workspace with floating translucent navigation and readable inner data surfaces.

### Admin requirements
- sidebar floats with outer canvas inset
- sidebar is light/translucent rather than dark
- navigation active state is subtle accent-tinted glass/solid inset
- metric cards have restrained depth; no loud gradients
- recent activity is the information anchor
- do not add charts just to make the redesign look richer

---

## B. AI Pass Workspace

Revise the visible `apps/demo-chat` product shell.

Target:
> approved company AI workspace, not a generic chat clone.

### Workspace requirements
- visible product is `AI Pass Workspace`
- light airy layout
- glass navigation/context controls
- generous conversation whitespace
- polished frosted composer
- `Protected by AI Pass` integrated with the composer
- destination/model context can remain the existing demo state
- no real provider/model API integration

### Composer
This should receive the most visual care.

It should include:
- strong frosted material
- visible but subtle blur
- soft shadow beneath the surface
- inner/edge highlight
- attachment/control buttons
- restrained accent send button
- integrated protection indicator

---

## C. Extension popup/intervention states

Revise all existing static/intervention components to the same light glass language.

Required states:
- scanning
- safe
- protected
- blocked

### Safe
- tiny, low-friction
- green semantic detail only

### Protected
- signature component
- frosted white glass card
- restrained amber semantic rim/accent
- clear `Send safely` primary action
- must look especially polished

### Blocked
- frosted light card
- restrained red semantic rim/accent
- calm explanation
- no alarm aesthetic

---

# 6. Material implementation requirement

Do not call something glass simply because opacity is below 1.

Key glass surfaces should combine:

```text
translucent white fill
+ backdrop-filter blur
+ saturation adjustment where useful
+ bright/fine border
+ inset top highlight
+ diffuse exterior shadow
+ background tonal variation behind the surface
```

If browser support lacks backdrop filtering, use a readable semi-opaque fallback.

---

# 7. Shared UI package

Update `packages/ui` so the revised system is reusable rather than implemented as per-app CSS hacks.

At minimum consolidate:
- canvas/background tokens
- text tokens
- accent tokens
- semantic status tokens
- glass levels
- readable surface level
- border tokens
- shadow tokens
- radius tokens
- motion timings
- common buttons
- badges/status pills
- glass panels
- intervention surface
- focus states

Remove obsolete dark-first tokens if no longer required by the demo.

Do not retain unused dark palette merely “for later” unless technically necessary.

---

# 8. Motion

Add only subtle stateful motion.

Expected examples:
- hover edge/shadow change
- button press feedback
- intervention card enter/exit
- status crossfade
- active nav transition
- composer focus transition

No:
- continuous floating
- animated mesh backgrounds
- pulsing protection badges
- parallax
- bouncing

Respect `prefers-reduced-motion`.

---

# 9. Responsive acceptance

Verify browser screenshots at:

```text
1600 px
1440 px
1280 px
1024 px
```

Also verify extension/intervention narrow width.

Check:
- no clipping
- no unintended horizontal scroll
- no collapsed unreadable glass
- sidebar/layout adapts sensibly
- composer remains usable

---

# 10. Visual evidence

The revision is not complete from source inspection alone.

Capture screenshots of at least:

1. Admin overview — 1440 px
2. AI Pass Workspace — 1440 px
3. Extension/intervention Protected state
4. Extension/intervention Blocked state
5. One 1024 px application view

If browser tooling permits comparison, inspect the screenshots before reporting completion and fix obvious issues.

---

# 11. Functional regression

This is a visual revision, so existing Phase 0–2 functionality must remain intact.

Run:
- build
- typecheck
- relevant unit tests
- any existing UI tests

Do not change policy-engine behavior merely to satisfy the redesign.

---

# 12. Revision acceptance gate

Report `PASS_UI_REVISION` only when:

- [ ] old Orbit branding is removed
- [ ] default UI is light
- [ ] dark-first shell is removed
- [ ] key glass surfaces visibly use blur/transparency/highlight/shadow
- [ ] atmospheric background allows glass to read clearly
- [ ] admin is visually coherent
- [ ] Workspace is visually coherent and not a ChatGPT clone
- [ ] composer looks like a signature product object
- [ ] Protected intervention is visually polished
- [ ] Blocked/Safe/Scanning states are consistent
- [ ] shared tokens/components reflect the new system
- [ ] accessibility/focus/reduced-motion behavior remains intact
- [ ] 1024–1600 widths are verified
- [ ] screenshots were inspected
- [ ] build/typecheck/tests pass
- [ ] no Phase 3 functionality was started

If any item fails, report `FAIL_UI_REVISION` and explain exactly what remains.

---

# 13. Scope stop

After this revision:

**STOP.**

Do not begin:
- Phase 3 interception
- real AI provider integrations
- multi-model routing
- backend expansion
- additional product features

The next implementation step must be explicitly requested by the user.
