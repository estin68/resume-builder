# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working in this repository.

## Repo purpose

`resume-builder` is a free, open-source, privacy-first resume builder that runs entirely in the browser: pick a template, edit profile/experience/skills inline, export as HTML or PDF. Public repo — **no personal career data belongs here**. Live demo: https://estin68.github.io/resume-builder/

## Hard constraints (do not violate)

- **No server, no build step.** Every page is a standalone static HTML file. Don't introduce a backend, bundler, or server-side rendering.
- **Dependency-free vanilla HTML/CSS/JS.** Keep the editor a single, portable file anyone can open offline. Don't add npm packages/frameworks without discussing first — this is a stated contributing constraint (see README.md), not a default to work around.
- **Local-first & private.** User data lives only in the browser (`localStorage`); nothing is uploaded, no tracking, no analytics.
- **No cross-origin scraping.** Nothing in this repo should fetch third-party sites (e.g. LinkedIn) from the browser — it doesn't work (CORS/auth) and isn't the design. Any AI-assisted analysis is done by generating a copy-pasteable prompt for the user's own AI tool, never a network call from the site.

## Project structure

```
resume-builder/
├── index.html      # Landing page + template gallery
├── guide.html      # Resume writing guide / tips
├── editor.html     # The shared resume editor (all templates/themes/fonts live here)
├── review.html     # Resume ↔ LinkedIn review feature (rule-based + AI-prompt generator)
├── README.md
└── LICENSE
```

Templates are CSS layout variants (`data-template`) combined with color theme variants (`data-theme`) inside a single editor engine — keeps the codebase small while offering multiple starting points.

## review.html specifics

See `docs/superpowers/specs/2026-08-20-linkedin-review-feature-design.md` for the full design. Key points to preserve when touching this page:

- LinkedIn content is always user-pasted, never fetched — the profile URL field is reference-only.
- Resume input prefers importing the existing `editor.html` localStorage draft; otherwise upload/paste, then cache under its own `localStorage` key.
- All comparison logic (consistency/gaps/keyword/tone) is rule-based vanilla JS. Deeper semantic judgment is deferred to the "copy AI-review prompt" button, not built into the page itself.

## Roadmap context

See README.md's Roadmap section for current status. Interview prep/quiz content and Cornell-notes study material are explicitly phase 2 / separate scope — don't fold them into resume-builder work unless asked.
