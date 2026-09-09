---
name: cv-copy
description: Edit the words on the CV Landscaping site — services, manifesto, reviews, contact details, and SEO/social meta. Use when Carter changes pricing, service offerings, phone or email, or when a new review comes in.
---

# CV Landscaping — copy

One file: `index.html`. Sections in page order:

```
#top         the intro / hero
#manifesto   the positioning paragraph
#services    the service rows
#work        the photo gallery
#reviews     client reviews
#contact     phone, email, service area
```

## Voice

Plain, concrete, Midland-specific. The site sells a crew that shows up and cuts
a straight line — not "premier outdoor living solutions." Short sentences. Name
real places (Community Baptist Church) when the photo shows one.

## When a review comes in

Reviews are hand-written into `#reviews`. Use the client's actual words, trimmed
— do not tidy their grammar into marketing copy. Keep the attribution format
consistent with the reviews already there.

## Contact changes ripple

A phone number or email change is never one edit. Check all of:

```bash
grep -n 'tel:\|mailto:\|@\|989' index.html
```

...plus the `og:` / `twitter:` meta in `<head>` and the JSON-LD block if present.

## Meta and social

`assets/og-cover.jpg` is the social preview. If the hero photo changes, the OG
cover usually should too — otherwise a shared link previews last season's lawn.

## Do not touch from here

Timing constants, `transform-origin` values, and the boot script — those belong
to `cv-boot-intro`. If a copy change makes the hero text longer, re-check the
intro at a narrow viewport, because the mark is sized off the measured box.
