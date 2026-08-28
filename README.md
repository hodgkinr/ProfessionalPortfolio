# BOBPE
## BOt-Based Personalized Education for Productivity Enhancement

This repository houses the content and supporting materials for the **BOBPE** professional portfolio website. BOBPE is an initiative focused on leveraging bot-based technologies to enable scalable personalized education and productivity enhancement, grounded in learning science and motivated by Bloom's 2-Sigma problem.

The repository is a **GitHub Pages/Jekyll site**, published from the `docs/` folder, plus supporting non-website material kept alongside it for provenance and works-in-progress.

---

## Repository Structure

```
.
├── Checklist.md          – content-tracking checklist for the live site
├── LOCAL_PREVIEW.md       – how to preview the Jekyll site locally via Docker
├── tree.txt               – generated directory listing (see below)
├── docs/                  – the live website (Jekyll source + built output)
│   ├── _config.yml
│   ├── CNAME
│   ├── index.md           – homepage
│   ├── mission.md
│   ├── contact.md
│   ├── assets/
│   ├── projects/          – one subfolder per project (canonical source)
│   └── _site/              – Jekyll's generated build output (not hand-edited)
├── misc/                  – non-website material (award nominations, etc.)
└── updates/                – design explorations and in-progress summaries
```

- `docs/index.md` / `docs/mission.md` / `docs/contact.md` – core site pages
- `docs/projects/` – individual project pages and supporting artifacts
- `docs/_site/` – Jekyll's rendered HTML output; regenerated automatically, not edited directly

---

## Website Content

### Homepage, Mission, Contact

- **Homepage** – `docs/index.md`
- **Mission** – `docs/mission.md`
- **Contact** – `docs/contact.md`

These files define the core framing and audience routing for the site.

---

## Projects

Project pages live under `docs/projects/`. Each project directory typically contains:
- A primary `.md` file used to generate the website page
- Optional PDFs, images, or raw READMEs for reference

The site (`docs/projects/index.md`) organizes projects into a broader **Educational AI Pipeline** plus three audience-based groupings.

### Educational AI Pipeline

- **Discovery** – `projects/discovery/discovery.md`
  Semantic search and exploration layer (embeddings, concept graphs, source-aware retrieval).

- **Forward LMS** – `projects/forward-lms/forward-lms.md`
  Lightweight course-generation and delivery system turning structured content into learner-facing modules.

- **AI Robustness Audit** – `projects/audit/audit.md`
  AI-informed course review system evaluating assessment robustness in the age of generative AI.

### For Students

- **Concept Inventory & Socratic Tutor Bots**
  `projects/concept-inventory-bots/concept-inventory-bots.md`
  Supporting material: `ASEE_2025_BUFFALO_WIP.pdf`

- **Code Narration & Interview Review Platform**
  `projects/code-narration-platform/code-narration-platform.md`
  Supporting material: `ASEE_RMS_2025___AI_Code_Review.pdf`

- **Computing & Engineering in the Age of AI**
  `projects/comp-eng-age-of-ai/comp-eng-age-of-ai.md`
  Supporting material:
  - `ASEE_2026___ASEN1030_Design (3).pdf`
  - `ASEE_rAIphie.pdf`
  - `living_poster.html` – standalone animated HTML presentation

---

### For Instructors

- **Canvas-Integrated Chatbots**
  `projects/canvas-integrated-chatbots/canvas-integrated-chatbots.md`

- **AI-Assisted LaTeX & Lab Report Grading**
  `projects/latex-lab-grading/latex-lab-grading.md`
  Supporting material: `raw_readme.md`

- **AI-Augmented Personalized Feedback**
  `projects/personalized-feedback/personalized-feedback.md`
  Supporting material: `images/`, `raw_readme.md`

---

### For Parents & the Public

- **Introduction to Generative AI (Coursera)**
  `projects/coursera/coursera.md`

- **Local Gemini Chatbot for Parents**
  `projects/gemini-parent-chatbot/gemini-parent-chatbot.md`
  Supporting material: `raw_readme.md`

- **AI Literacy Workshops, Community Talks, and Podcasts**
  `projects/ai-literacy-workshops/ai-literacy-workshops.md`

---

## Intentionally Excluded / In-Progress Frameworks

The following directories are included for continuity and future expansion, but are **not currently surfaced** on the public website navigation:

- `docs/projects/buffalo/`
- `docs/projects/pollenators/`
- `docs/projects/charge/`

These represent broader research or organizational frameworks that will be incorporated once they are more fully developed or externally documented.

---

## Non-Website Material

### `misc/`

Explicitly **not** part of the website (see `misc/README.md`). Currently holds:

- `Award_Nominations/CU_award_spring_2026/` – drafts for the CU Boulder AI Recognition Award nomination ("Structured AI Integration to Strengthen Student Reasoning & Validation"), including nomination details/draft text, an emails log, and Claude Design poster prompt notes.

### `updates/`

Working area for design explorations and in-progress summaries not yet folded into the site:

- `site-scheme-from-stitch/stitch_interactive_concept_map/` – AI-generated (Stitch) UI concept explorations, each with a standalone `code.html` and preview screenshot: AI Learning Assistant, Educator Insights Hub, Interactive Concept Map, and Student Mastery Dashboard (all CU Boulder-themed), plus a `cognitive_sanctuary/DESIGN.md` concept.
- `working-project-summaries/audit-lms-semantic.md` – in-progress project summary.

---

## Notes on Usage

- Markdown files in `docs/` are the **canonical source of truth** for website copy.
- `docs/_site/` is Jekyll's generated output — do not hand-edit; it's rebuilt from the source markdown.
- PDFs, images, and raw READMEs are included for reference and provenance.
- `misc/` and `updates/` are working/reference material and are not part of the published site.

---

## Local Preview

This site can be previewed locally with Docker.

From the repo root, run:

```bash
cd /path/to/ProfessionalPortfolio && docker run --rm -it -p 4000:4000 -v "$PWD/docs":/srv/jekyll jekyll/jekyll:pages bash -lc "gem install webrick && jekyll serve --host 0.0.0.0 --force_polling"
```

Then open:

```text
http://127.0.0.1:4000
```

Press `Ctrl+C` in the terminal to stop the server.

Notes:

- The site source lives in `docs/`
- The Jekyll config is `docs/_config.yml`
- This repo does not currently include a local Bundler setup, so Docker is the simplest preview method

See the file [LOCAL_PREVIEW.md](LOCAL_PREVIEW.md)

---

## Status

BOBPE is an active, evolving initiative. Projects range from classroom deployments to research prototypes and public-facing outreach. Each project page documents its scope, audience, and maturity level.

---

*This repository reflects an ongoing effort to align advances in AI with established learning science, enabling more human-centered, personalized education at scale.*
