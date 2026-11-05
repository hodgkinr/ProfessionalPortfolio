# BOBPE Professional Portfolio

## BOt-Based Personalized Education for Productivity Enhancement

This repository publishes [bobpe.com](https://bobpe.com), Bobby Hodgkinson's professional portfolio and BOBPE project site. The current site has two complementary parts:

1. **The BOBPE project website** — a Jekyll site describing the initiative, its learning-science foundation, and student-, instructor-, and public-facing projects.
2. **The promotion evidence dashboard** — a standalone static application that presents a public, candidate-authored map of accomplishments for promotion to Teaching Professor.

The repository also retains supporting materials, working notes, and design explorations that are not part of the public navigation.

## Current Project State

The BOBPE website and its project pages are live and continue to evolve. The portfolio now extends beyond the original project-oriented Jekyll site through the promotion dashboard at [bobpe.com/dashboard/](https://bobpe.com/dashboard/).

The dashboard currently includes:

- A promotion evidence map organized around impact in the disciplinary unit, at CU Boulder, and in national or international teaching discussions
- **54 public evidence summaries** covering curriculum development, student engagement, leadership and service, scholarship of teaching and learning, outreach, and practitioner experience
- Evidence mapped to CEAS promotion domains and Smead Teaching Quality Framework (TQF) dimensions
- A teaching statement
- A leadership and service statement
- A readable TQF reference
- A promotion-format curriculum vitae (PDF)

The dashboard is a public working snapshot, not an official dossier or institutional evaluation. Its detailed supporting evidence, internal notes, open questions, and verification records remain in the private promotion repository. The dashboard pages also carry `noindex,nofollow` metadata; that discourages search indexing but should not be treated as access control.

Projects represented across the site range from active course deployments and durable curriculum systems to research-informed prototypes, institutional service, public outreach, and future work. Claims in the dashboard distinguish deployed work from pilots and planned directions within the individual evidence descriptions.

## Published Site Architecture

GitHub Pages publishes the contents of `docs/` using the custom domain in `docs/CNAME`.

### 1. Jekyll site

The main site is generated from Markdown and configured by `docs/_config.yml`.

- `docs/index.md` — homepage and BOBPE framing
- `docs/mission.md` — mission and principles
- `docs/contact.md` — contact page
- `docs/projects/index.md` — project index and audience groupings
- `docs/projects/` — canonical project-page Markdown and supporting artifacts
- `docs/privacy/augmented-grader.md` — public privacy policy for the Augmented Grader
- `docs/assets/` — Jekyll styling
- `docs/_site/` — generated Jekyll output; do not hand-edit

### 2. Promotion evidence dashboard

`docs/dashboard/` is a prebuilt client-side application deployed alongside the Jekyll site.

- `docs/dashboard/index.html` — dashboard route shell
- `docs/dashboard/teaching-statement/index.html` — teaching statement route shell
- `docs/dashboard/leadership-service-statement/index.html` — leadership and service statement route shell
- `docs/dashboard/tqf-source/index.html` — TQF reference route shell
- `docs/dashboard/assets/` — generated JavaScript and CSS bundles
- `docs/dashboard/cv/` — public promotion-format CV
- `docs/dashboard/source/catalog.md` — minimized public snapshot of the evidence catalog
- `docs/dashboard/source/map.md` — public promotion/TQF bucket map and classification audit
- `docs/dashboard/source/teaching-statement.md` — readable teaching statement source snapshot
- `docs/dashboard/source/leadership-service-statement.md` — readable leadership/service statement source snapshot

The bundled assets and route-shell HTML are deployment output and should not be edited by hand. This repository does not currently contain the dashboard's unbundled application source or build configuration. The Markdown files under `docs/dashboard/source/` are readable public snapshots; the private promotion repository remains the source of truth for the full evidence record and should be reconciled before the dashboard is regenerated.

## Project Portfolio

The main project index organizes work around an educational AI pipeline and three audiences.

### Educational AI pipeline

- **Discovery** — semantic search and exploration using embeddings, concept graphs, and source-aware retrieval
- **Forward LMS** — lightweight generation and delivery of structured learner-facing modules
- **AI Robustness Audit** — AI-informed course review focused on quality and assessment robustness

### For students

- Concept Inventory and Socratic Tutor Bots
- Code Narration and Interview Review Platform
- Computing and Engineering in the Age of AI

### For instructors

- Canvas-Integrated Chatbots
- AI-Assisted LaTeX and Lab Report Grading
- AI-Augmented Personalized Feedback

### For parents and the public

- Introduction to Generative AI on Coursera
- Local Gemini Chatbot for Parents
- AI Literacy Workshops, Community Talks, and Podcasts

The `buffalo`, `pollenators`, and `charge` project directories preserve broader research or organizational frameworks that are not currently emphasized in public site navigation.

## Repository Layout

```text
.
├── README.md                     project overview and maintenance guidance
├── Checklist.md                  legacy/site content-tracking checklist
├── LOCAL_PREVIEW.md              local Jekyll preview instructions
├── tree.txt                      generated directory listing
├── docs/                         GitHub Pages publication root
│   ├── _config.yml               Jekyll configuration
│   ├── CNAME                     custom domain
│   ├── index.md                  BOBPE homepage
│   ├── mission.md
│   ├── contact.md
│   ├── assets/                   Jekyll styles
│   ├── projects/                 canonical project pages and artifacts
│   ├── privacy/                  public privacy documentation
│   ├── dashboard/                deployed promotion dashboard and public snapshots
│   └── _site/                    generated Jekyll output
├── misc/                         non-site supporting material and award drafts
└── updates/                      design explorations and working summaries
```

## Content and Source-of-Truth Rules

- Edit Markdown under `docs/` for the main Jekyll site; do not hand-edit `docs/_site/`.
- Treat `docs/projects/` as the canonical source for public project descriptions in this repository.
- Treat `docs/dashboard/assets/` and the dashboard route-shell HTML files as generated deployment artifacts.
- Treat `docs/dashboard/source/` as a minimized public export, not the complete promotion evidence base.
- Reconcile dashboard claims with the private promotion repository before rebuilding or publishing a new dashboard version.
- Preserve PDFs, images, and raw READMEs because they provide provenance and supporting context.
- Keep private or sensitive supporting evidence out of the public `docs/` publication tree.
- `misc/` and `updates/` are working/reference areas and are not part of normal public navigation.

## Local Preview

The Jekyll portion of the site can be previewed with Docker. From the repository root:

```bash
docker run --rm -it -p 4000:4000 \
  -v "$PWD/docs":/srv/jekyll \
  jekyll/jekyll:pages \
  bash -lc "gem install webrick && jekyll serve --host 0.0.0.0 --force_polling"
```

Then open [http://127.0.0.1:4000](http://127.0.0.1:4000). Press `Ctrl+C` to stop the preview.

The dashboard is already compiled into `docs/dashboard/` and uses root-relative `/dashboard/...` asset paths. Preview it through a web server rooted at `docs/`; opening its HTML files directly from the filesystem will not reproduce the deployed routing and asset behavior.

This repository does not currently include a `Gemfile`, so Docker is the simplest supported Jekyll preview path. See `LOCAL_PREVIEW.md` for the shorter preview guide.

## Maintenance Notes

- `Checklist.md` predates the promotion dashboard and primarily tracks project-site content; it is not a complete status record for the repository.
- When dashboard content changes, update both the deployed build and the readable public snapshots together.
- Verify public links, route behavior, PDF availability, and the boundary between public and private evidence before publishing.
- Update this README when the site's architecture, dashboard contents, or source-of-truth workflow changes.

---

BOBPE is an ongoing effort to align advances in AI with established learning science so personalized, human-centered education can operate at meaningful scale while preserving human judgment, student agency, and responsible use.
