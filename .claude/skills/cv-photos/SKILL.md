---
name: cv-photos
description: Add, swap, or reorder the client photos on the CV Landscaping site — the hero shot, the services rows, and the work gallery. Use when Carter sends new job photos, when a photo needs replacing, or when the same shot is showing up twice on the page.
---

# CV Landscaping — photos

All photos live in `assets/`, referenced from `index.html`. No build step.

## Naming

Photos come off the phone and keep their camera names — `IMG_7343.jpg`,
`IMG_2519.jpg`. **Keep it that way.** Renaming to descriptive slugs breaks the
match against Carter's originals when he sends a replacement for "the one of the
church."

## Where photos appear

| Slot | Selector | Notes |
|---|---|---|
| Hero | `.intro__bg img` | `fetchpriority="high"` — the boot gate waits on this one decoding |
| Services rows | `.svc-row__img` | one per service |
| Services stack | the `.is-on` sibling set | first has `.is-on` |
| Work gallery | `figure.c-a` … `.c-e` | fixed five-slot layout |

## The one hard rule: the hero photo appears once

The most recent change to this site was *"Stop the hero photo repeating further
down the page."* Whatever is in `.intro__bg img` must not appear again in
`#services` or `#work`. Before adding a photo, grep it:

```bash
grep -c 'IMG_7343.jpg' index.html   # hero should be 1
```

## There is already a spare photo library in the repo

These are committed but **unused** — check here before asking Carter for more:

```
IMG_1870  IMG_2823  IMG_2824  IMG_4244  IMG_5347  IMG_7325
estate-lawn  fall-equipment  hero-mow  home-front
```

Confirm current usage before trusting that list:

```bash
for f in assets/*.jpg; do echo "$(grep -c "$(basename $f)" index.html)  $f"; done | sort -n
```

## Alt text

Every photo has real alt text describing the actual property, in the site's
voice — *"A striped lawn running back to planted beds and a covered patio"*,
not "lawn care photo". Decorative duplicates in the stack use `alt=""`.
Match that.

## Loading

- Hero: `fetchpriority="high"`, no `loading` attribute.
- Everything else: `loading="lazy"`.

Do not put `loading="lazy"` on the hero — the boot gate waits on its decode, so
lazy-loading it stalls the intro to the 3600ms ceiling every time.
