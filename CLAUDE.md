# jacob-lingo.github.io

Personal site for Jacob Lingo, computer engineer (Orlando, FL). Static HTML/CSS,
no build step. Deployed by GitHub Pages from `main` at repo root — pushing to
`main` publishes to https://jacob-lingo.github.io.

## Local development

```
python3 -m http.server 8000
```

Then open http://localhost:8000. There is no build, bundler, or package manager.
Do not add one without asking.

## Structure

```
index.html              single page, all content
assets/css/main.css     Astral template stylesheet — VENDOR, do not edit
assets/css/custom.css   all custom styles go here (loaded after main.css)
assets/js/              jQuery + Astral template scripts — VENDOR, do not edit
images/                 photo, favicons, resume PDF
```

The theme is **Astral by HTML5 UP** (CCA 3.0). Vendor files are `main.css`,
`noscript.css`, and everything in `assets/js/`. Override in `custom.css` rather
than modifying them, so the template stays diffable against upstream.

## Architecture gotcha: the panel system

`assets/js/main.js` implements single-panel routing. Only one
`<article class="panel">` inside `#main` is visible at a time; nav clicks change
`window.location.hash` and a `hashchange` handler swaps panels with a height
transition.

Consequences to know before changing markup:

- `$panels` is captured **once at page load** as `$main.children('.panel')`.
  New panels must be direct children of `#main` and present in the initial HTML.
  Panels injected later will not register.
- `#main` gets `max-height`/`min-height` set during the transition and cleared
  afterward, so long panels scroll normally. Long content is fine.
- Any `<a href="#panel-id">` works, not just nav links — the hashchange handler
  catches it.
- Nav is icon-only and horizontal. Five items fit down to the 360px breakpoint
  (~275px of 360). A sixth will not.

This architecture is a candidate for replacement (see Open questions).

## Design tokens

Derived from `main.css` — match these, don't introduce new ones:

| Role | Value |
|---|---|
| Body text | `#777777` |
| Headings, strong | `#363636` |
| Muted / meta labels | `#aaaaaa` |
| Hairline rules | `#dddddd` |
| Buttons | `#222222` (hover `#333`, alt `#777`) |
| Panel background | `#ffffff` |
| Font | Source Sans Pro, 300 body / 400 headings |
| Border radius | **0 everywhere.** No pills, no rounded cards. |

Base font-size is large and steps down by breakpoint (20pt → 15pt → 14pt → 12pt
→ 11pt at 360px). Size everything in `em` and check the 736px breakpoint, where
small labels get too small if they scale twice.

## Content rules

- **Never invent accomplishments, metrics, dates, or technologies.** If a detail
  isn't in the resume PDF or explicitly provided, ask rather than fill it in.
- The Mitsubishi Power customer portal is an **internal, non-public product**.
  Do not add technical detail beyond what already appears on the public resume.
  No screenshots, no repo links, no architecture specifics.
- Voice: direct, plain, concrete. No "passionate about," no "cutting-edge," no
  filler superlatives. Short sentences are fine.

## Known issues

- **Contact form is broken.** `<form action="mailto:..." method="post">` — modern
  browsers do not support POST to a `mailto:` target. Needs a real endpoint
  (Formspree, Netlify Forms, or a mailto link with `method="get"` as a fallback).
- Form points at `ja250373@ucf.edu`; the resume lists `jacobadamlingo@gmail.com`.
  Pick one.
- `<meta viewport>` includes `user-scalable=no`, which blocks pinch-zoom. This is
  an accessibility problem inherited from the template and should be removed.
- Resume PDF says MS expected **May 2028**. Confirm before it circulates further.
- No project links anywhere — every project entry currently dead-ends.

## Open questions

- Replace the panel/hash architecture with a plain scrolling single page?
  Panels give no deep-linkable project URLs and ship ~100KB of jQuery for
  navigation alone.
- Add screenshots or diagrams to project entries. Biggest content gap.
