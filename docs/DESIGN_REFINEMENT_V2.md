# AI Pass — Design Refinement V2

This file **supersedes conflicting visual guidance** in earlier design documents. It exists because the first light-glass revision was cleaner but still looked like a generic AI SaaS template.

For any UI task, treat this file as the highest-priority visual direction together with the user-supplied reference images.

## 1. Target: Frosted Editorial Utility

The product should feel like a real material object floating in space, not a collection of pastel SaaS cards.

The strongest reference is the user-provided finance dashboard image with:
- one large smoky/frosted main glass panel
- a narrow detached floating navigation rail
- background light and depth visibly bleeding through the glass
- warm neutral ambience
- restrained orange accent
- mostly monochrome icons
- asymmetrical, editorial composition
- simple but characterful typography

Translate those qualities into AI Pass. Do not copy the banking layout, content, card artwork, or proprietary details.

## 2. Explicitly remove from the current UI

The following are rejected:
- lavender/purple/indigo page tint
- purple icon backgrounds
- purple send button
- robot iconography
- shield icon used as a brand motif
- generic sparkle/AI icons as the main identity
- large permanent ChatGPT-style recent-conversation sidebar
- identical rounded white cards repeated everywhere
- generic blue/purple gradient blobs
- visual language that could belong to any AI SaaS startup

Purple/lilac/indigo must not remain as the product accent or ambient hue.

Security icons are allowed only where semantically necessary, e.g. a tiny lock/check/alert on a protection state. They are not the brand.

## 3. Color direction

Use warm neutral glass with one confident warm accent.

Suggested starting tokens:

```css
--canvas: #EDEDE9;
--canvas-light: #F7F6F2;
--canvas-cool: #E9ECEC;

--ink: #24231F;
--text-2: #65645F;
--text-3: #92908A;

--glass: rgba(255,255,255,.36);
--glass-strong: rgba(255,255,255,.56);
--glass-readable: rgba(250,250,247,.72);
--glass-line: rgba(255,255,255,.70);
--neutral-line: rgba(37,36,31,.10);

--brand: #F4772C;
--brand-hover: #E8671D;
--brand-soft: rgba(244,119,44,.12);

--safe: #2E8B67;
--protect: #B9822A;
--block: #C8544F;
```

The brand orange should appear in small, high-value places only:
- active navigation marker
- primary send/action button
- selected control
- small brand mark

Do not tint large backgrounds orange.

## 4. Background and light

The favorite reference works because there is a meaningful world behind the glass.

For the application, create that depth without requiring a literal stock photo:
- warm gray/off-white base
- large blurred dark-neutral shadow/light masses
- one soft warm amber light source
- one cooler gray/blue-green light source if needed
- subtle grain/noise
- strong enough tonal variation that backdrop blur is clearly visible

Think abstract architectural light, not pastel gradient wallpaper.

Avoid lavender mesh gradients.

A future marketing/demo mode may use a photographic or rendered environment, but the core app should remain usable with a CSS/abstract background.

## 5. Typography — move away from generic Geist

Primary choice: **Instrument Sans**.

Why:
- precise but less generic than Geist/Inter
- contemporary neo-grotesque character
- variable widths/weights
- real italics
- open-source and available through Google Fonts

Use it as the UI family unless implementation constraints make it unsuitable.

Typography direction:
- body/UI: Instrument Sans 400–500
- navigation/control labels: 500–550
- section headings: 500–600
- page title: optionally Instrument Sans Medium Italic for a restrained editorial touch inspired by the reference
- metrics: medium weight with tabular numerals

Do not use huge bold AI-startup headings.

Fallback if Instrument Sans is unavailable: **Manrope**. Do not fall back to Geist without documenting why.

## 6. Brand treatment

Do not invent a robot mascot or a complex AI/security logo.

For this demo use either:
1. a clean `AI Pass` wordmark; or
2. `AI Pass` plus one very simple abstract geometric mark.

Preferred temporary brand gesture:
- text wordmark
- small orange dot/aperture accent

Avoid:
- robot head
- shield + sparkle
- magic wand
- neural-network nodes
- generic four-point AI star as primary identity

## 7. Navigation — major structural change

The wide ChatGPT-like sidebar is not the desired direction.

Prefer a **detached narrow glass rail**, approximately 64–80 px on desktop, visually inspired by the favorite reference.

It may contain:
- wordmark/mark at top
- new chat / overview action
- history/activity icon
- policies icon
- devices/settings icon where relevant
- user/account at bottom

Labels can appear via tooltip or an expanded transient panel.

If persistent text navigation is necessary for the admin, use a compact secondary layer rather than a full-height 250 px opaque sidebar.

The main workspace should feel larger and more cinematic as a result.

## 8. Main application frame

Prefer **one dominant glass frame** with internal hierarchy instead of many disconnected white cards.

Characteristics:
- 1 px luminous edge
- visible backdrop blur
- medium transparency so background light reads through it
- large soft shadow
- 24–32 px radius for the main frame
- internal readable surfaces can be more opaque

This main frame is the visual anchor.

## 9. Workspace composition

AI Pass Workspace should not look like ChatGPT recolored.

Recommended structure:

```text
[narrow floating rail]   [single large glass workspace frame]
                         [minimal top utility strip]
                         [conversation / welcome state]
                         [floating material composer]
```

For chat/history:
- do not permanently reserve a wide left column
- history can open as a glass drawer/popover
- conversation text should sit directly in the main frame with more breathing room

Top utility strip:
- destination/model context
- private/approved state
- minimal menu

No purple badges.

## 10. Composer — signature object

The composer should feel tactile and materially distinct.

Use:
- stronger smoky-white glass
- subtle inner highlight
- heavier diffuse shadow beneath
- thin neutral line
- brand-orange send action
- monochrome attachment/document icons
- compact protection state integrated into the object

`Protected by AI Pass` should be quiet text/status, not a green SaaS pill floating above the composer.

When protection state changes, use small semantic cues rather than recoloring the entire composer.

## 11. Admin composition

Use the same material language but do not copy the chat layout.

Recommended:
- narrow floating rail
- one large translucent dashboard plane
- asymmetrical bento composition inside
- 1 large activity surface
- 2–4 smaller supporting modules
- stronger hierarchy than equal KPI cards

The favorite finance reference is useful here: varied module sizes, clear visual anchor, compact controls, and a strong single frame.

Do not fill the page with six identical white statistic cards.

## 12. Icons

Use Lucide or another restrained outline family in near-black/gray.

Rules:
- no purple icon tiles
- no colored square behind every icon
- inactive icons monochrome
- active navigation may use the brand orange
- semantic colors only for actual safe/protected/blocked meaning

## 13. Glass recipe V2

The current UI is too milky/opaque. Allow more background to show through.

Suggested dominant-frame material:

```css
background: linear-gradient(
  135deg,
  rgba(255,255,255,.46),
  rgba(255,255,255,.28)
);
backdrop-filter: blur(30px) saturate(118%);
-webkit-backdrop-filter: blur(30px) saturate(118%);
border: 1px solid rgba(255,255,255,.68);
box-shadow:
  0 30px 80px rgba(49,45,38,.14),
  0 8px 24px rgba(49,45,38,.07),
  inset 0 1px 0 rgba(255,255,255,.80);
```

Use readable inner surfaces where text density requires them.

## 14. Neumorphism — only as micro-detail

The user also likes the polished soft-depth quality of neumorphism.

Use it sparingly for:
- compact segmented controls
- small circular icon buttons
- inset toggles
- selected tabs

Do not build the whole UI out of raised gray neumorphic blocks.

## 15. Motion

Use material motion rather than generic SaaS animation:
- rail item highlight glides subtly
- glass drawer slides/fades with slight scale
- composer lifts 1–2 px on focus
- send button compresses physically on press
- protected/block intervention emerges from the composer region

No floating blobs, pulsing stars, or continuous gradient animation.

## 16. Acceptance test for originality

The revision fails if a reviewer can describe it as:
- “ChatGPT but glassy”
- “a shadcn AI dashboard”
- “purple AI SaaS”
- “generic glassmorphism template”

It should instead be describable as:

> A warm, material, editorial glass workspace with a distinctive floating rail and restrained orange signal system.

## 17. Required comparison before completion

Before declaring the next UI refinement complete, capture screenshots and compare them against:
1. the current AI Pass light revision
2. the user-supplied favorite glass finance dashboard
3. the target rules in this file

Explicitly verify:
- no purple/lavender ambient hue
- no purple icons/buttons
- no robot/shield-as-brand
- Instrument Sans (or documented alternative) is active
- narrow detached navigation rail exists
- one dominant glass frame exists
- background visibly participates in the glass material
- workspace no longer reads like a standard ChatGPT clone
- composer feels like a signature material object
