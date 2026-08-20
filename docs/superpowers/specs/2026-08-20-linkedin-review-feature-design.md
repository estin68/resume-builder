# Resume ↔ LinkedIn Review Feature — Design

Status: approved
Date: 2026-08-20

## Purpose

Add a new page, `review.html`, that lets any visitor compare their resume
(built here or uploaded/pasted) against their LinkedIn profile content
(pasted manually) and get a findings report plus a ready-made AI-review
prompt — fully client-side, consistent with this repo's local-first,
dependency-free, no-server philosophy.

Out of scope: anything involving personal career data for the maintainer
specifically (that lives in the separate, private `personal_resume` repo
and is not touched by this feature).

## Hard platform constraints (why the design looks like this)

- **No server, no build step.** Vanilla HTML/CSS/JS only, one more static
  page alongside `index.html` / `editor.html` / `guide.html`.
- **No cross-origin LinkedIn fetch.** Browser JS cannot fetch a LinkedIn
  profile from a pasted URL — LinkedIn requires login and blocks CORS.
  Building a server-side fetcher would require standing up a backend
  (breaks the no-server model) and risks LinkedIn's anti-scraping ToS if
  used at scale by arbitrary visitors. So the LinkedIn URL field is
  reference-only (used in the generated AI prompt); actual LinkedIn
  content must be pasted by the user.
- **No filesystem access.** A webpage cannot write exports to, or
  silently read from, a fixed local folder — there is no general
  filesystem API for that. "Load available resume automatically" is
  implemented via `localStorage` caching (same mechanism `editor.html`
  already uses for autosave), not a real directory.
- **No LLM/AI backend.** All analysis is rule-based (string/keyword
  heuristics, no external calls). Deeper semantic judgment (tone,
  nuanced consistency) is deferred to an AI-review prompt the user copies
  into their own AI tool of choice — no network call originates from the
  site itself.

## Page: `review.html`

New page in the site nav, alongside `index.html` / `editor.html` /
`guide.html`.

### Inputs

**Resume** (in priority order):
1. If an `editor.html` draft exists in `localStorage`, offer a one-click
   "Import from editor draft" button.
2. Otherwise/also: upload the tool's own self-contained exported HTML
   (parsed via `DOMParser`, reusing the editor's existing content
   structure/classes) or paste plain text.
3. Whatever resume content is loaded gets cached under its own
   `localStorage` key (e.g. `reviewResumeDraft`) so it auto-reloads next
   visit, with a "use different resume" control to clear/replace it.

**LinkedIn**:
- Profile URL field — reference-only, not fetched, included in the
  generated AI prompt for context.
- Paste fields: Headline, About, Experience entries (repeatable, same
  add/remove pattern as the editor's experience blocks), Skills.

**Target role**:
- Dropdown reusing the site's existing four example categories (QA
  Engineer, Senior Full-Stack Engineer, Engineering Manager, Fresh
  Graduate) plus a "Custom" option for a typed role name.
- Free-text custom keywords field, always available alongside the
  dropdown, to add extra target keywords regardless of selected role.

### Analysis (rule-based, vanilla JS, no dependencies)

Four categories, each finding tagged `category` / `severity` /
resume-text / linkedin-text / note:

1. **Consistency** — per-experience-entry title/date comparison between
   resume and pasted LinkedIn experience entries, using a small
   hand-written word-overlap/fuzzy-match function (no library). Flags
   likely mismatches (different title or date range for what looks like
   the same role).
2. **Gaps** — term/phrase set-difference between resume and LinkedIn text
   blocks (headline, about, each experience entry, skills). Flags
   content present on one side but missing from the other.
3. **Keyword/ATS** — match combined resume+LinkedIn text against the
   selected target role's built-in keyword list plus custom keywords.
   Reports coverage percentage and missing terms.
4. **Tone/branding** — heuristic word-list check: does the headline's
   seniority language (e.g. "Lead", "Senior", "Manager") match
   leadership-language presence in About/Experience (e.g. "led",
   "managed", "mentored")? Flags mismatch in either direction.

These are mechanical checks only — no semantic judgment. Deeper judgment
is explicitly deferred to the AI-prompt step below.

### Output

Rendered inline on the same page, below the inputs, after clicking
"Review":

1. **Summary bar** — counts by category/severity.
2. **Findings list** — category, severity, resume text vs. LinkedIn text
   side-by-side, rule-based note.
3. **Copy AI-review prompt button** — assembles a prompt template
   embedding the user's resume text, LinkedIn text, target role, and the
   rule-based findings, asking an AI assistant to do the deeper
   consistency/gap/keyword/tone judgment. Copies to clipboard via
   `navigator.clipboard`; no network call from the site.
4. **Save report as HTML button** — mirrors `editor.html`'s existing
   "Save HTML" pattern (Blob + download) so the user can keep a copy of
   the findings.

### Error handling

- Missing resume or LinkedIn input → "Review" button stays disabled with
  an inline hint, not a silent no-op.
- Uploaded file isn't recognizable as this tool's exported HTML → fall
  back to treating its content as plain text.

### Testing

No build/test tooling exists in this repo currently. Verify manually:
run `review.html` end-to-end with one real resume/LinkedIn pair and one
synthetic example, confirm findings render and the AI-prompt/save-report
buttons work.

## Companion change: `CLAUDE.md`

Add a new, sanitized `CLAUDE.md` at the repo root (this repo currently
has none) covering: repo purpose, structure, the "dependency-free vanilla
JS, no server" contributing constraint already stated in `README.md`, and
guidance on the new `review.html` feature. Contains no personal career
data — this repo is public.
