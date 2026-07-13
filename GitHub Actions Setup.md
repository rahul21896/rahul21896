# GitHub Actions Setup

Deep-dive documentation for the four workflows in `.github/workflows/` of
[`rahul21896/rahul21896`](https://github.com/rahul21896/rahul21896), plus banner guidance.

> **Note:** this repository's default branch is **`master`** (not `main`). Every branch
> reference below reflects that, and the workflow YAMLs are written for it.

---

## Table of Contents

1. [Prerequisite for ALL workflows](#1-prerequisite-for-all-workflows)
2. [snake.yml — Contribution Snake](#2-snakeyml--contribution-snake)
3. [metrics.yml — Detailed Metrics](#3-metricsyml--detailed-metrics)
4. [activity.yml — Recent Activity](#4-activityyml--recent-activity)
5. [blog-posts.yml — Latest Blog Posts](#5-blog-postsyml--latest-blog-posts)
6. [Troubleshooting](#6-troubleshooting)
7. [Cron quick reference](#7-cron-quick-reference)
8. [Banner recommendations](#8-banner-recommendations)

---

## 1. Prerequisite for ALL workflows

Every workflow in this repo **writes back to the repository** (a branch, an SVG file, or the
README itself). GitHub's default token permissions are read-only, so this is a one-time,
mandatory step:

1. Open **github.com/rahul21896/rahul21896** → **Settings** → **Actions** → **General**.
2. Scroll to **Workflow permissions**.
3. Select **"Read and write permissions"**.
4. Click **Save**.

Each workflow also declares `permissions: contents: write` at the YAML level, but the
repository setting above must allow it — the effective permission is the *intersection* of
the two. If you skip this step, every workflow fails with
`Permission denied to github-actions[bot]` (see [Troubleshooting](#6-troubleshooting)).

There is no organization involved here (personal repo), so no org-level override applies.

---

## 2. snake.yml — Contribution Snake

**File:** `.github/workflows/snake.yml` · **Workflow name:** `Generate Snake Animation`

### What it does

1. **`Platane/snk/svg-only@v3`** reads the public contribution graph for
   `${{ github.repository_owner }}` (resolves to `rahul21896`) and renders two animated SVGs
   into `dist/`:
   - `github-contribution-grid-snake.svg` — light palette
   - `github-contribution-grid-snake-dark.svg?palette=github-dark` — dark palette
2. **`crazy-max/ghaction-github-pages@v4`** publishes the `dist/` directory to the
   **`output`** branch (`target_branch: output`, `build_dir: dist`). The branch is created
   automatically on the first run and force-updated on every run — never commit anything
   else to it.

### Triggers

| Trigger | Value |
|---|---|
| Schedule | `0 0 * * *` — daily at 00:00 UTC (05:30 IST) |
| Push | every push to `master` |
| Manual | `workflow_dispatch` (Actions tab → Generate Snake Animation → **Run workflow**) |

### Token

Uses the built-in **`GITHUB_TOKEN`** (`secrets.GITHUB_TOKEN`). **No setup required** beyond
the workflow-permissions prerequisite in section 1.

### Where the README consumes it

The `<picture>` block in the "🐍 Contribution Snake" section of `README.md` loads the SVGs
straight from the raw content of the `output` branch:

```
https://raw.githubusercontent.com/rahul21896/rahul21896/output/github-contribution-grid-snake.svg
https://raw.githubusercontent.com/rahul21896/rahul21896/output/github-contribution-grid-snake-dark.svg
```

The dark variant is served via `<source media="(prefers-color-scheme: dark)">`, so viewers
get the palette matching their GitHub theme. Until the first successful run, these URLs 404
and the README shows a broken image — trigger the workflow manually right after pushing.

---

## 3. metrics.yml — Detailed Metrics

**File:** `.github/workflows/metrics.yml` · **Workflow name:** `GitHub Metrics`

### What it does

Runs **`lowlighter/metrics@latest`**, which generates a rich stats infographic and commits
it as **`github-metrics.svg`** to the **`master`** branch. The README embeds it in the
"📈 Detailed Metrics" section via:

```
https://raw.githubusercontent.com/rahul21896/rahul21896/master/github-metrics.svg
```

### Triggers

| Trigger | Value |
|---|---|
| Schedule | `0 6 * * *` — daily at 06:00 UTC (11:30 IST) |
| Manual | `workflow_dispatch` |

### REQUIRED SETUP — `METRICS_TOKEN` secret

Unlike the other workflows, lowlighter/metrics calls the GitHub API on your behalf and
**needs a classic Personal Access Token**. The built-in `GITHUB_TOKEN` is not sufficient
(it cannot read account-level data like your full contribution/user profile).

**Step 1 — create the token:**

1. Go to **github.com/settings/tokens** → **Generate new token** → **Generate new token (classic)**.
2. Note: `METRICS_TOKEN for rahul21896/rahul21896 metrics workflow`.
3. Expiration: your call — 90 days is a sane default; set a calendar reminder to rotate it.
4. Scopes: check **`repo`** and **`read:user`**. Nothing else.
5. **Generate token** and copy it immediately (shown only once).

**Step 2 — add it as a repository secret:**

1. Open **github.com/rahul21896/rahul21896** → **Settings** → **Secrets and variables** → **Actions**.
2. **New repository secret**.
3. Name: `METRICS_TOKEN` (exact, case-sensitive — the workflow references `secrets.METRICS_TOKEN`).
4. Value: paste the token → **Add secret**.

**Step 3 — run it:** Actions tab → GitHub Metrics → **Run workflow**. The first run commits
`github-metrics.svg` to `master` and the README image goes live.

### Configuration reference (as written in the YAML)

| Option | Value | What it does |
|---|---|---|
| `token` | `${{ secrets.METRICS_TOKEN }}` | Classic PAT used for all API reads |
| `user` | `rahul21896` | Account to render metrics for |
| `template` | `classic` | Standard single-column infographic layout |
| `filename` | `github-metrics.svg` | Output file committed to `master` |
| `base` | `header, activity, community, repositories, metadata` | Built-in sections: name/avatar header, activity counters, followers/following, repo stats, and generation metadata footer |
| `config_timezone` | `Asia/Kolkata` | Renders all timestamps in IST instead of UTC |
| `plugin_languages` | `yes` | Adds a most-used-languages breakdown |
| `plugin_languages_limit` | `8` | Caps the language list at 8 entries |
| `plugin_languages_sections` | `most-used` | Shows only the "most used" view (no "recently used") |
| `plugin_isocalendar` | `yes` | Adds an isometric 3D contribution calendar |
| `plugin_isocalendar_duration` | `half-year` | Calendar covers the last 6 months |
| `plugin_lines` | `yes` | Adds lines-of-code added/removed history |

The action pins `@latest` intentionally — lowlighter publishes `latest` as the stable
release channel. If a future upstream change breaks a plugin, pin a release tag
(e.g. `lowlighter/metrics@v3.34`) instead.

---

## 4. activity.yml — Recent Activity

**File:** `.github/workflows/activity.yml` · **Workflow name:** `Recent Activity`

### What it does

1. `actions/checkout@v4` checks out the repo.
2. **`jamesgeorge007/github-activity-readme@master`** reads your recent **public** GitHub
   events (commits, PRs, issues, comments) and rewrites the README **between the markers**:

   ```html
   <!--START_SECTION:activity-->
   <!--END_SECTION:activity-->
   ```

   Both markers must remain in `README.md` exactly as written — the "⚡ Recent GitHub
   Activity" section already contains them. Configured with `MAX_LINES: 8` (at most 8
   items) and commit message `docs: update recent GitHub activity`.

### Triggers

| Trigger | Value |
|---|---|
| Schedule | `0 */6 * * *` — every 6 hours, on the hour (00:00, 06:00, 12:00, 18:00 UTC) |
| Manual | `workflow_dispatch` |

### Token

Built-in **`GITHUB_TOKEN`** only. **No secrets to configure.** The commit appears as
`github-actions[bot]`, which by design does **not** re-trigger the snake workflow's
push-to-master trigger (GitHub suppresses workflow runs caused by `GITHUB_TOKEN` commits,
preventing loops).

If there is no new public activity since the last run, the action makes no commit — a green
run with no diff is normal, not a failure.

---

## 5. blog-posts.yml — Latest Blog Posts

**File:** `.github/workflows/blog-posts.yml` · **Workflow name:** `Latest Blog Posts`

### What it does

**`gautamkrishnar/blog-post-workflow@v1`** fetches posts from the RSS feed(s) in
`feed_list` and rewrites the README between:

```html
<!-- BLOG-POST-LIST:START -->
<!-- BLOG-POST-LIST:END -->
```

Configured with `max_post_count: 5` and the built-in `GITHUB_TOKEN` (`gh_token`) — no
secrets needed.

### Current state: intentionally dormant

The workflow ships **manual-only** (`workflow_dispatch`, cron commented out) because
`feed_list` is a placeholder (`https://your-blog.example.com/feed`). Running it as-is
would fail or inject nothing useful. Do not enable the cron until the feed is real.

### How to enable

**Step 1 — set your real feed URL(s)** in `.github/workflows/blog-posts.yml`
(comma-separated if more than one):

| Platform | Feed URL format |
|---|---|
| Medium | `https://medium.com/feed/@your-username` |
| Dev.to | `https://dev.to/feed/your-username` |
| WordPress | `https://your-blog.com/feed` |
| Hashnode | `https://your-blog.hashnode.dev/rss.xml` |

**Step 2 — uncomment the schedule block:**

```yaml
on:
  workflow_dispatch:
  schedule:
    - cron: "0 * * * *" # every hour
```

**Step 3 — commit, push, then run it once manually** (Actions → Latest Blog Posts →
**Run workflow**) to verify the feed parses before waiting on the cron.

Hourly is the action author's recommended cadence; drop to `0 */6 * * *` if you publish
infrequently and want fewer bot commits.

---

## 6. Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| Snake workflow fails / renders an empty grid | Account has zero contributions in the last year — `Platane/snk` has nothing to animate on brand-new or inactive accounts | Not an issue for this account (it has history). If it ever occurs: make any public contribution, then re-run |
| `Permission denied to github-actions[bot]` on any workflow | Workflow permissions still at default read-only | Section 1: Settings → Actions → General → Workflow permissions → **Read and write permissions** → Save, then re-run |
| Metrics fails with `401 Unauthorized` / "Bad credentials" | `METRICS_TOKEN` missing, expired, revoked, or lacking `repo` + `read:user` scopes | Regenerate the classic PAT with both scopes and update the `METRICS_TOKEN` secret (Settings → Secrets and variables → Actions) |
| Activity run is green but README unchanged | `<!--START_SECTION:activity-->` / `<!--END_SECTION:activity-->` markers missing or altered, **or** simply no new public events since last run | Restore the markers verbatim; the no-new-events case is normal and resolves itself |
| Blog posts run fails or lists nothing | `feed_list` still the placeholder, feed URL wrong, or `BLOG-POST-LIST` markers missing | Section 5: set a real feed, keep both markers, run manually to verify |
| README images stale after a successful run | GitHub proxies all README images through its **camo** cache | Wait (cache TTL is short, usually minutes) or force-refresh: `curl -X PURGE "https://camo.githubusercontent.com/<image-hash>"` using the camo URL from the image's "Copy image address" |
| Snake/metrics images 404 in the README | The consuming URLs point at files that don't exist until the **first successful run** | Trigger each workflow manually once via `workflow_dispatch` right after pushing |

---

## 7. Cron quick reference

All GitHub Actions cron schedules run in **UTC**. IST = UTC+5:30.

| Workflow | Cron | Meaning | IST equivalent |
|---|---|---|---|
| snake.yml | `0 0 * * *` | Daily at 00:00 UTC | 05:30 IST |
| metrics.yml | `0 6 * * *` | Daily at 06:00 UTC | 11:30 IST |
| activity.yml | `0 */6 * * *` | Every 6 hours at minute 0 | 05:30, 11:30, 17:30, 23:30 IST |
| blog-posts.yml (commented) | `0 * * * *` | Every hour at minute 0 | on the half-hour IST |

Field order: `minute hour day-of-month month day-of-week`. Two caveats worth knowing:

- **Scheduled runs are best-effort.** During peak load GitHub may delay or occasionally
  skip a cron tick. Every workflow here also has `workflow_dispatch` as a manual fallback.
- **60-day inactivity rule.** GitHub disables scheduled workflows in repos with no activity
  for 60 days. Any commit (including the bot commits these workflows make to `master`)
  resets the clock, so in practice this profile repo keeps itself alive.

---

## 8. Banner recommendations

### Current setup (recommended: keep it)

The README's header and footer banners are **capsule-render** URLs
(`capsule-render.vercel.app`) — animated waving gradients in the profile's accent colors
(`#3B82F6` → `#10B981`), with the name and title rendered as URL parameters. Advantages:

- **No static file to create or maintain** — edit the URL query string to change text,
  colors, height, or animation.
- Always crisp at any width (SVG), and the white text works on both GitHub themes.

### If you want a custom static banner later

1. **File location:** save it as `assets/banner.png` in this repo (the `assets/` directory
   already exists).
2. **Dimensions:** **1280×400 minimum**; export at **2560×800** for retina displays
   (README images render at ~1280px wide on desktop, so 2× keeps it sharp).
3. **Design for dark mode first:** most GitHub users browse in dark theme. Avoid pure-white
   backgrounds; stick to the profile's palette (`#3B82F6`, `#10B981`, dark neutrals like
   `#0F172A`/`#1F2937`) so it matches the badges and cards. Verify it in both themes before
   committing.
4. **Tools:** **Figma** (search the community for a free "GitHub header" / "GitHub profile
   banner" template and restyle it) or **Canva** (LinkedIn-banner templates resize well to
   1280×400).
5. **Swap it in:** replace the capsule-render `<img>` at the **top** of `README.md` with a
   relative path so it needs no external service:

   ```html
   <div align="center">
     <img src="assets/banner.png" alt="Rahul Dhamecha — Senior Full Stack Developer" width="100%" />
   </div>
   ```

   Leave the footer capsule-render wave as-is, or remove it for a cleaner ending — your
   call. Relative paths resolve correctly on the profile page once the file is on `master`.
