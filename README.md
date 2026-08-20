# 📄 Resume Builder

A **free, open-source, privacy-first resume builder** that runs entirely in your browser. Pick a template, edit your profile / experience / skills inline, and export a polished, ATS-friendly resume as **HTML** or **PDF** — no sign-up, no server, no tracking.

**🔗 Live demo:** https://estin68.github.io/resume-builder/

---

## ✨ Features

- **Template gallery** — choose from multiple layout + color combinations (Classic, Minimal, Modern) before you start editing.
- **Inline WYSIWYG editor** — click **✏️ Edit** and type directly on the resume; no forms, no JSON, no drag-and-drop builder complexity.
- **Add / remove / reorder** — add new job entries, skill tags, bullet points, education, certifications, or volunteering items; move experience blocks up/down; delete anything you don't need.
- **Theme & font picker** — 14 color themes and 15 font stacks, mix-and-match with any layout template.
- **One-page fit gauge** — a live indicator shows how full the page is so your resume stays print-ready on a single A4 page.
- **Save as HTML** — exports a fully self-contained `.html` file (styles + your content baked in) you can reopen, keep editing, or send to someone else.
- **Export to PDF** — uses the browser's native print dialog ("Save as PDF") for a clean, one-page PDF output.
- **Local-first & private** — your draft autosaves to your browser's `localStorage` only. Nothing is uploaded anywhere. Closing the tab does not lose your work; clearing the browser data will.
- **Resume writing guide** — a short, practical guide on structuring a resume, quantifying impact, and staying ATS-friendly.

> 🚧 **Not included yet (planned for a later phase):** interview quiz/prep tools and Cornell-notes style study content. This repo is scoped to **template selection + editing + export** only.

---

## 🚀 Quick Start

### Option 1 — Use the hosted version
Just open the live demo link above. Everything runs client-side; no installation needed.

### Option 2 — Run locally
```bash
git clone https://github.com/estin68/resume-builder.git
cd resume-builder
# then simply open index.html in your browser, or serve it locally:
python3 -m http.server 8080
# visit http://localhost:8080
```

---

## 🖱️ How it works

1. **Browse templates** on the home page (`index.html`) — each card previews a layout + color theme.
2. Click **Use this template** → opens the editor pre-loaded with that style and sample placeholder content.
3. Click **✏️ Edit** in the toolbar and replace the placeholder text with your own name, summary, skills, and experience.
4. Use **+ Add Experience**, **+ add** (under skill/education/list sections), and the ✕ / ↑ / ↓ controls to fully customize the content and ordering.
5. Adjust **theme**, **font**, and **layout template** any time from the toolbar dropdowns.
6. Click **💾 Save HTML** to download a self-contained resume file, or **🖨️ Export PDF** to print/save as a one-page PDF.

Your in-progress draft is auto-saved to your browser's local storage, so you can safely refresh or come back later on the same device/browser.

---

## 🗂️ Project Structure

```
resume-builder/
├── index.html      # Landing page + template gallery
├── guide.html      # Resume writing guide / tips
├── editor.html     # The shared resume editor (all templates/themes/fonts live here)
├── README.md
└── LICENSE
```

Templates are implemented as CSS layout variants (`data-template`) combined with color theme variants (`data-theme`) inside a single editor engine — this keeps the codebase small while still offering multiple distinct-looking starting points.

---

## 🗺️ Roadmap

- [x] Template gallery + shared inline editor
- [x] Save HTML / Export PDF
- [x] Resume writing guide
- [ ] More layout templates (single-column, timeline style)
- [ ] Import existing resume (paste text / upload) to prefill sections
- [ ] Interview prep quizzes & Cornell-notes study content *(phase 2, separate scope)*

---

## 🤝 Contributing

Issues and PRs are welcome — especially new template layouts/themes, accessibility improvements, and bug fixes. Please keep the editor dependency-free (vanilla HTML/CSS/JS) so it stays a single, portable file that anyone can open offline.

---

## 📜 License

Released under the [MIT License](LICENSE).
