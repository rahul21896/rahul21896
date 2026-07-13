# Recommended Repository Structure

A repository organization strategy for a recruiter-facing GitHub profile. Recruiters and hiring managers spend 30–90 seconds on a profile: they read the README, scan the pinned repos, and click at most one or two of them. Everything below is organized around making those 90 seconds count.

Companion documents: `README.md` (the profile itself), `GitHub Actions Setup.md` (workflow/secret setup), `Customization Guide.md` (placeholder map).

---

## 1. Pinned Repositories

GitHub allows **6 pins**. Pins are the first repos anyone sees, so they should be exclusively your own work, ordered strongest-first.

**How to pin (2026 UI):** go to `github.com/rahul21896` → the **"Customize your pins"** link at the top-right of the pinned section (or the "Pinned" section's edit control) → check up to 6 repos → drag to reorder → Save.

### Pin today (all four real, non-fork repos)

| Order | Repo | Why it earns a pin |
|-------|------|--------------------|
| 1 | `headless-wordpress` | Your strongest public sample — headless WordPress + React proves the exact decoupled-CMS architecture featured on the README project card. |
| 2 | `mern-demo` | Shows JavaScript full-stack range (MongoDB/Express/React/Node) beyond the PHP core competency. |
| 3 | `data-science` | Backs up the AI/Python "Current Focus" claims in the README with actual code. |
| 4 | `slideshow` | Small but real frontend JavaScript work; fine as a fourth pin until a showcase repo replaces it. |

That leaves **two empty pin slots** — reserve them for the showcase repos in Section 2. As each showcase repo ships, it should displace the weakest pin (start with `slideshow`, then `data-science` unless it has been fleshed out).

### Do not pin forks

`laravel`, `my-portfolio`, `flask-ecommerce`, and `FlexSlider` are forks. To a recruiter, a pinned fork reads as padding — the code isn't yours, the commit history isn't yours, and GitHub labels it "Forked from …" right on the card. Recommendations:

- Never pin them.
- If a fork exists only because you once tried it out and it has no commits of yours, **delete it** (Settings → General → Danger Zone → "Delete this repository"). Deleting a fork does not affect the upstream project.
- If a fork carries your own commits you want to keep (e.g. `my-portfolio`), keep it but make sure the four real repos and the profile README visually dominate the profile.

---

## 2. Showcase Repositories to Create

**These repos do not exist yet.** They are the recommended next builds — each one converts a "Private Client Project" claim on the README into verifiable public code. Build them in this order; each is 1–3 weekends of effort.

### 2.1 `laravel-api-starter`

- **Purpose:** A production-ready Laravel API boilerplate — the distilled version of the patterns you already use in the admission portal, CRM, and ERP work.
- **What it proves to recruiters:** Senior-level Laravel architecture you can show, not just describe: service layer, repository pattern, form requests, API resources, versioned routes, queue-driven jobs, and a real test suite. This is the single highest-leverage repo for a Laravel-focused job search.
- **Suggested contents:** Laravel 11+, Sanctum auth, role/permission scaffolding, Redis cache + queue config, PHPUnit feature tests, Docker Compose for local dev, GitHub Actions CI (lint + tests), OpenAPI spec, seeded demo data.
- **Topics:** `laravel`, `php`, `rest-api`, `boilerplate`, `docker`, `phpunit`.

### 2.2 `wp-plugin-boilerplate`

- **Purpose:** A modern WordPress/WooCommerce plugin skeleton reflecting how you structure the plugin suites you build for clients.
- **What it proves to recruiters:** That your "6+ years WordPress/WooCommerce" claim comes with engineering discipline — namespaced PHP, PSR-4 autoloading via Composer, hook-driven architecture, a WooCommerce integration example, settings page, and uninstall cleanup. Most WP code on GitHub is messy; a clean skeleton stands out sharply.
- **Suggested contents:** PSR-4 structure, one working WooCommerce extension example (e.g. a custom checkout field), Gutenberg block registration example, WP-CLI command, PHPCS with WordPress coding standards, PHPUnit + WP test scaffold.
- **Topics:** `wordpress`, `woocommerce`, `wordpress-plugin`, `php`, `boilerplate`.

### 2.3 `ai-document-search-demo`

- **Purpose:** A sanitized, public reimplementation of your FAISS/LangChain document-search work — synthetic sample documents, no client data.
- **What it proves to recruiters:** That the AI/RAG line on your profile is hands-on: embedding pipeline → FAISS vector store → retrieval API with cited sources. AI-adjacent roles will click this first.
- **Suggested contents:** FastAPI service, LangChain ingestion pipeline, FAISS index persisted to disk, OpenAI (or a local sentence-transformers fallback so the demo runs without an API key), small seed corpus, `docker compose up` one-command start, short architecture diagram in the README.
- **Topics:** `python`, `fastapi`, `langchain`, `faiss`, `rag`, `vector-search`.

**Rule for all showcase repos:** each must run from a fresh clone with the documented commands, include screenshots or a short GIF, and get its "Live Demo" badge wired into the README project cards (currently `🔧 REPLACE-ME` placeholders).

---

## 3. Naming Conventions

Consistent names make a profile scannable in the repo list view, where only the name and one-line description are visible.

**Rules:**

- **kebab-case, all lowercase** — `laravel-api-starter`, never `LaravelAPIStarter` or `laravel_api_starter`. (Existing exception: the fork `FlexSlider` keeps its upstream name; forks are exempt.)
- **Lead with the technology or domain prefix** so related repos sort together:
  - `laravel-*` — Laravel packages, starters, demos
  - `wp-plugin-*` / `wp-theme-*` — WordPress work
  - `react-*` / `nextjs-*` — frontend projects
  - `api-*` — language-agnostic API projects
  - `aws-*` — infrastructure, IaC, deployment tooling
  - `ai-*` — LLM/RAG/vector-search projects
- **Name the thing, not the attempt.** Avoid `test`, `demo1`, `new-project`, `misc`, `stuff`. A name should tell a stranger what is inside without clicking. (`mern-demo` is borderline acceptable because "MERN" carries the meaning; `slideshow` and `data-science` are vague — see Section 4.)
- **Keep names under ~30 characters** so they don't truncate on pinned-repo cards.

**Every public repo, no exceptions, needs:**

1. **A one-line description** (the About field) — verb-first, ≤ 120 chars: "Production-ready Laravel 11 API boilerplate with service layer, Sanctum auth, and Docker."
2. **3–6 topics** for discoverability and search.
3. **Its own README** with this structure:
   - Banner or screenshot at the top (assets live in the repo, not hotlinked)
   - **What** — one paragraph: what it is
   - **Why** — one paragraph: the problem it solves / what it demonstrates
   - **Stack** — badge row or bullet list
   - **Setup** — copy-pasteable commands, verified from a fresh clone
   - **Screenshots** — at least one; a GIF for anything interactive
4. **A LICENSE file** — MIT is the sensible default for showcase code. A repo without a license is legally "all rights reserved," which discourages the stars and forks that make a profile look alive.

---

## 4. Repository Hygiene (do this week)

Verified via the GitHub API on 2026-07-13: `headless-wordpress` has a description; **`mern-demo`, `slideshow`, and `data-science` have no description and no topics.** Empty About fields are the single cheapest fix on the whole profile.

### Add descriptions and topics

Via the UI: repo page → gear icon next to **About** (right sidebar) → set Description, Website, Topics → Save changes.

Via `gh` CLI (suggested copy — edit to taste):

```bash
gh repo edit rahul21896/mern-demo \
  --description "Full-stack MERN application demo — MongoDB, Express, React, Node" \
  --add-topic mern --add-topic react --add-topic nodejs --add-topic mongodb --add-topic express

gh repo edit rahul21896/slideshow \
  --description "Lightweight vanilla JavaScript image slideshow component" \
  --add-topic javascript --add-topic slideshow --add-topic frontend --add-topic ui-component

gh repo edit rahul21896/data-science \
  --description "Data science notebooks and experiments — Python, pandas, ML fundamentals" \
  --add-topic python --add-topic data-science --add-topic machine-learning --add-topic jupyter

# headless-wordpress already has a description; add topics for parity:
gh repo edit rahul21896/headless-wordpress \
  --add-topic wordpress --add-topic headless-cms --add-topic react --add-topic rest-api --add-topic php
```

### Give each real repo a README that follows the Section 3 structure

`headless-wordpress` deserves this first — it is pin #1 and linked from the profile README's project card.

### Archive, don't delete, stale experiments

If a repo is genuinely dead but shows real work (old commit history counts toward your contribution graph and languages), archive it rather than deleting: repo → Settings → General → scroll to **Danger Zone** → **"Archive this repository."** Archived repos get a read-only banner, drop out of "active project" impressions, and can be unarchived at any time. Delete only empty or zero-value repos.

---

## 5. Organization Strategy

### Personal account is correct — don't create an org yet

For an individual job search, everything should live on `rahul21896`. A one-person GitHub organization with three repos looks like an empty office. Create an org only when one of these becomes true:

- You ship an open-source project with multiple maintainers.
- You need to separate freelance **client work** with its own access control, billing, or private-repo policies — an org gives per-repo teams and keeps client code contractually isolated from your personal account.
- A product/brand outgrows your personal namespace.

### Client work stays private and off the profile

Never publish client code under your account, even sanitized, without written permission. The README already handles this correctly: project cards describe the private systems and the "Private Client Project" badge sets expectations. The showcase repos in Section 2 are the compliant way to make that experience verifiable — clean-room reimplementations of the *patterns*, with synthetic data.

### Topics are your search surface

GitHub search and the Explore page index topics. Recruiters sourcing for "laravel bengaluru" or browsing the `laravel` topic can only find repos that are tagged. Standardize on lowercase topic slugs and reuse the same core set (`laravel`, `php`, `wordpress`, `woocommerce`, `react`, `aws`, `docker`, `python`, `rag`) so your work clusters together.

### The profile repo is the front door

`rahul21896/rahul21896` (default branch `master`) renders its README at the top of your profile — treat it as the homepage everything else links from:

- Project cards link out to the real repos; the "More public code" line covers the rest.
- Keep the four workflows healthy (`snake.yml`, `metrics.yml`, `activity.yml`, `blog-posts.yml`) so the profile visibly self-updates — a dead widget is worse than no widget. Requirements: the `METRICS_TOKEN` secret for metrics, and Settings → Actions → General → Workflow permissions → **"Read and write permissions"** on the profile repo (details in `GitHub Actions Setup.md`).
- When a showcase repo ships, update three places in the same commit: pin it, add/upgrade its README project card, and replace the corresponding `🔧 REPLACE-ME` Live Demo badge.

### Maintenance cadence

- **Monthly (5 min):** confirm the workflows are green (repo → Actions tab), check that pinned repos still reflect your best work.
- **Per new repo:** description + topics + README + LICENSE before the first push goes public — never let a bare repo sit on the profile.
- **Quarterly:** re-evaluate pins against Section 1's order; archive anything you haven't touched in a year and wouldn't show in an interview.
