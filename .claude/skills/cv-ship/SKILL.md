---
name: cv-ship
description: Ship a CV Landscaping change to the live Vercel site at cvlandscapingmi.vercel.app, and keep the old draft files out of the deploy. Use when pushing changes live, when a draft page leaks onto the client's site, or when confused about which index file is the real one.
---

# CV Landscaping — shipping

Live at **https://cvlandscapingmi.vercel.app**, deployed from
**`EthanLongMedia/cvlandscapingmi`**.

## Which repo is the real one

`EthanLongMedia/CV-Landscaping-LLC` is **empty** — zero commits. It is not the
site and nothing should be pushed there. The site is `cvlandscapingmi`. If a
tool or session hands you the LLC repo, stop and switch.

## Which file is the real one

`index.html` is the site. These are **old drafts kept for reference only**:

```
index-old.html          index-apple-v1.html          index-prev-0830.html
```

They are listed in `.vercelignore` so Vercel never serves them. **Any new draft
you keep must be added to `.vercelignore` in the same commit** — otherwise it
deploys to a guessable URL on the client's domain.

Also ignored: `.claude`, `cv-landscaping-standalone.html`, `CV-Landscaping.html`
(the last two are gitignored as well — the 5MB single-file export).

## Before pushing

```bash
python3 -m http.server 8000     # hard-reload; the intro only runs on a fresh load
```

Check, in order:

1. The intro plays through — ink lifts *after* the leaf settles, no pulse at the
   nav handoff, no page shift when the scrollbar returns.
2. The hero photo appears exactly once on the page.
3. Narrow viewport (390px) — the mark is sized off a measured box, so long copy
   can push it.
4. `prefers-reduced-motion` on — the boot should be skipped entirely.

Then push. Vercel auto-deploys from the default branch.

## Local launch config

`.claude/launch.json` points at `/Users/admin/cv-landscaping` — that is Ethan's
Mac path, not this checkout. Serve the repo directory directly instead.
