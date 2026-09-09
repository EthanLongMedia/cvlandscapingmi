---
name: cv-boot-intro
description: Edit the CV Landscaping opening sequence — the ink curtain, the leaf sway, the wordmark, and the mark's flight into the nav bar. Use when changing intro timing, the loading screen, the animated leaf, the hero photo fade-in, or when the intro flashes, pulses, stutters, or shifts the page.
---

# CV Landscaping — the boot intro

Everything below lives in `index.html`. There is no build step: edit, save, reload.

## What the sequence actually is

1. `.boot` (`#boot`) covers the page in ink. Inside it: `.boot__stripes` wipe,
   the two-layer mark, and `.boot__word` (`wordmark-white.png`).
2. The stripes wipe uncovers the mark (`.05s` delay + `.65s` run).
3. The **leaf dips once and settles** — `.bootleaf`, `@keyframes leafSettle`,
   `2.1s linear .3s both`. It only runs once `.boot` has `.is-ready`.
4. The ink lifts, the hero photo fades up, and the hero mark flies into
   `.nav__mark`, which gets `.landed`.

## The leaf — how it is built

The leaf is **two PNG layers on the same 1328×752 canvas**, stacked in `.boot__in`:

- `mark-base.png` — the logo with the leaf silhouette **cut out**
- `mark-leaf-root.png` — the leaf **plus a root**: the logo's own white within
  90px of the tip, so it travels with the leaf and no seam opens at the joint

`transform-origin: 72.89% 48.27%` is the pivot at the leaf's stem. Do not round
it — it was measured against the artwork.

**`mark-leaf.png` is the rootless leaf and is NOT wired up.** Use
`mark-leaf-root.png`. Swapping to the rootless one opens a white gap mid-sway.

Sway writes to the leaf's own `transform`. The flight into the nav writes to a
different element. Keep them on separate elements or they fight.

## The timing gate — do not turn these into a plain delay

```
FLOOR = 2450   CEIL = 3600   FADE = 700
```

The ink is a **cover for the hero photo decoding**, not a fixed wait. It lifts
the moment `.intro__bg img` has decoded, but:

- never before `FLOOR` — 2450ms covers the wipe (50 + 650) **and** the leaf
  settling (300 + 2100 = 2400). Drop FLOOR below 2400 and the ink lifts
  mid-shake.
- never after `CEIL` — the bad-signal ceiling.

**If you change `leafSettle`'s duration or delay, raise `FLOOR` to match.**
That is the single most common way to break this intro.

## Rules that look like bugs but are deliberate

- `html.booting .intro__bg img { opacity: 0 }` — `booting` is dropped inside
  `clearBoot()`, *before* the fade, on purpose. That is what lets the photo fade
  up while the ink fades down.
- `.nav__mark` has **no opacity transition** — only `transform`. Fading it in
  cross-faded it against the flying mark and read as a pulse.
- `.lenis.lenis-stopped { overflow: visible }` overrides Lenis's own
  `overflow:hidden`. Lenis's version removed the scrollbar for two seconds and
  put it back — a visible page shift. `lenis.stop()` already swallows
  wheel/touch, so the lock is not needed.
- `window.__cvRemeasure()` (defined at the bottom as `init`) is called once more
  right before the ink lifts, after webfonts have settled, so the hero measures
  the box it will actually use.

## Reduced motion

`prefers-reduced-motion` sets `boot.style.display = 'none'` outright and
`.boot.is-ready .bootleaf { animation: none }`. Any new intro motion needs a
matching opt-out — check it with the emulation setting in DevTools.

## Checking it

`python3 -m http.server 8000`, then hard-reload (the intro is gated on a fresh
load). Watch for: ink lifting mid-leaf-shake, a pulse at the nav handoff, and
the page jumping when the scrollbar returns.
