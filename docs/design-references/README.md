# AI Pass — Design Reference Images

Store only a small number of deliberate visual references in this folder so future Codex UI tasks can inspect the same images rather than relying on verbal descriptions alone.

## Recommended files

Add the user-provided references with stable names such as:

```text
primary-glass-dashboard.png
light-glass-card.png
light-glass-editorial.png
current-ai-pass-before-v2.png
```

`primary-glass-dashboard.png` is the highest-priority reference: the finance-dashboard concept with a smoky transparent main plane, detached vertical rail, warm background light, monochrome controls, and restrained orange accent.

## How Codex should use them

For a UI task, Codex should inspect the actual images and use them to understand:
- material/transparency
- depth
- proportions
- navigation shape
- typography attitude
- color restraint
- visual hierarchy

Do not copy banking content, proprietary assets, logos, card artwork, or an exact layout.

The images are art-direction references, not implementation assets.

## Priority

1. `primary-glass-dashboard.png`
2. `docs/DESIGN_REFINEMENT_V2.md`
3. `docs/DESIGN_SYSTEM.md`
4. other reference images

If an older written design rule conflicts with `DESIGN_REFINEMENT_V2.md`, follow the refinement document.
