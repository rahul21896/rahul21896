# Customization Guide

Complete reference for personalizing this profile README (`github.com/rahul21896/rahul21896`, default branch **`master`**).

Every placeholder in `README.md` is marked with a `🔧 REPLACE-ME` comment. Section 1 maps each one to an exact search string. Everything else in this guide is optional tuning.

---

## 1. Placeholder Map

Search for the **exact string** in the left column (in `README.md` unless stated otherwise) and replace as described. Counts are exact — use find-and-replace-all where a string appears more than once.

| Placeholder | Exact search string | Occurrences | Replace with |
|---|---|---|---|
| Email | `your.email@example.com` | 2 (hero badges + Connect With Me, both inside `mailto:`) | Your real email address |
| Portfolio URL | `https://your-portfolio-website.com` | 2 (hero badges + Connect With Me) | Your portfolio URL, or delete both `<a>…</a>` badge blocks if you have none |
| Resume link | `<a href="#"><img src="https://img.shields.io/badge/Resume` | 1 | Replace `href="#"` with a public URL to your resume PDF (Google Drive share link, portfolio path, etc.), or delete the badge |
| Twitter/X handle | `https://twitter.com/your-handle` | 1 | Your X profile URL, or delete the badge |
| LeetCode | `https://leetcode.com/your-username` | 1 | Your LeetCode profile URL, or delete the badge |
| Stack Overflow | `https://stackoverflow.com/users/your-id` | 1 | Your numeric SO user URL, e.g. `https://stackoverflow.com/users/1234567/rahul` |
| Medium | `https://medium.com/@your-username` | 1 | Your Medium profile URL (keep the `@`) |
| Dev.to | `https://dev.to/your-username` | 1 | Your Dev.to profile URL |
| Buy Me a Coffee | `https://buymeacoffee.com/your-username` | 1 | Your BMC username, or delete the badge |
| GitHub Sponsors | `https://github.com/sponsors/rahul21896` | 1 | Link is already correct, **but it only works after you enroll** at github.com → Settings → Sponsors (or github.com/sponsors). Delete the badge if you don't plan to enroll |
| Live Demo badges | `<a href="#"><img src="https://img.shields.io/badge/Live_Demo-🔧_REPLACE--ME-3B82F6?style=flat-square" alt="Live demo placeholder" /></a>` | 8 (one per project card) | Per card: set `href` to the live URL and change the badge text `🔧_REPLACE--ME` to `View` (note: `--` renders a literal `-` in shields.io) — or delete the whole `<a>…</a>` if no demo exists. Deleting is the honest default for private client work |
| Blog RSS feed | `https://your-blog.example.com/feed` in `.github/workflows/blog-posts.yml` | 1 | Your real RSS feed URL (Medium: `https://medium.com/feed/@user` · Dev.to: `https://dev.to/feed/user` · WordPress: `https://blog.example.com/feed`). Then **uncomment the `schedule:` block** in the same file so it runs hourly instead of manual-only |

Not a placeholder, but verify: LinkedIn (`linkedin.com/in/rahuldhamecha`) and all GitHub links are already real.

---

## 2. Theme & Colors

### Accent system

The entire profile uses two accents. Change them everywhere consistently or not at all:

| Color | Hex | Used in |
|---|---|---|
| Blue | `#3B82F6` | Header/footer gradient start, typing text, badges, streak ring, activity-graph lines |
| Green | `#10B981` | Gradient end, badges, streak fire, activity-graph line color |

In URLs the hex appears **without** the `#` (e.g. `color=0:3B82F6,100:10B981`, `ring=3B82F6`). A global find-and-replace on `3B82F6` and `10B981` re-skins everything except shields that use their brand colors.

### Header / footer banner (capsule-render)

Two `capsule-render.vercel.app/api` images: header (top of README) and footer (bottom). Key query params:

| Param | Current (header) | Options |
|---|---|---|
| `type` | `waving` | `venom`, `blur`, `rect`, `soft`, `rounded`, `shark`, `slice`, `cylinder`, `egg`, `transparent` |
| `color` | `0:3B82F6,100:10B981` | Gradient as `position:HEX` stops; or a single hex; or `auto` for random |
| `height` | `220` (footer: `140`) | Pixels |
| `text` | `Rahul%20Dhamecha` | URL-encoded (`%20` = space) |
| `fontSize` / `fontColor` | `62` / `ffffff` | — |
| `desc` / `descSize` / `descAlignY` | tagline / `18` / `58` | Sub-heading line |
| `animation` | `fadeIn` | `blink`, `blinking`, `scaleIn`, `twinkling` |

### Typing animation (readme-typing-svg.demolab.com)

The rotating lines live in the `lines=` param — **semicolon-separated, URL-encoded**:

- space → `+` (or `%20`), `|` → `%7C`, `+` → `%2B`, `&` → `%26`, `/` → `%2F`

Current value:

```
lines=Senior+Full+Stack+Developer+%7C+7.5%2B+Years;Laravel+%26+PHP+Enterprise+Applications;AWS+Cloud+%7C+Docker+%7C+CI%2FCD;WordPress+%26+WooCommerce+Specialist;AI+%26+LLM+Integrations
```

Other params: `font`, `weight`, `size`, `duration` (ms per line), `pause` (ms between lines), `color` (hex, no `#`), `width` (increase if a long line clips).

### Card themes

Stats cards use `theme=transparent` so they render cleanly in both GitHub light and dark mode — recommended default. To change:

- **github-profile-summary-cards** (`?theme=`): `transparent`, `tokyonight`, `github_dark`, `github`, `dracula`, `radical`, `gruvbox`, `nord_dark`, `solarized`, `vue` — change on all 5 card URLs together.
- **streak-stats.demolab.com** (`?theme=`): the README uses `transparent` plus explicit accents (`ring`, `fire`, `currStreakLabel`, `sideLabels`); a named theme like `tokyonight` overrides those.
- **github-readme-activity-graph** uses explicit params instead of a theme: `bg_color`, `color`, `line`, `point` (hex, no `#`), `area=true`, `hide_border=true`.
- **Trophies** (`gh-trophy.cdnsoft.net`, `?theme=`): currently `algolia`; alternatives include `flat`, `onedark`, `dracula`, `nord`, `discord`, `dark_dimmed`.
- **Dev quote** (`quotes-github-readme.vercel.app`, `?theme=`): currently `tokyonight`.

Fixed-theme cards (`tokyonight`, `algolia`) look fine in dark mode but show a dark box on light mode — acceptable, just be aware.

---

## 3. Self-Hosting github-readme-stats (Optional Upgrade)

The popular `github-readme-stats.vercel.app` public instance is **currently down — verified 503 `DEPLOYMENT_PAUSED` on 2026-07-13**. That is why this README uses `github-profile-summary-cards.vercel.app` + `streak-stats.demolab.com` instead. The public instance also rate-limits heavily even when up, so the project's own recommendation is to self-host. If you want the classic stats/top-langs cards:

1. **Fork** `github.com/anuraghazra/github-readme-stats` to your account.
2. Create a **classic PAT** at github.com → Settings → Developer settings → Personal access tokens → Tokens (classic) → *Generate new token (classic)*. No scopes are required for public-repo stats; add `repo` if you want private contributions counted.
3. Go to **vercel.com/new**, sign in with GitHub, and **Import** your fork.
4. Before deploying, expand **Environment Variables** and add: name `PAT_1`, value = the token from step 2.
5. Click **Deploy** and note your domain, e.g. `rahul-stats.vercel.app`.
   - If the deploy fails with a `maxDuration` error on the Hobby plan, edit `vercel.json` in your fork and lower `maxDuration` (e.g. to `10`), then redeploy.

Then swap into the README (e.g. alongside or replacing the summary-cards block in **GitHub Stats**):

```html
<img src="https://YOUR-APP.vercel.app/api?username=rahul21896&show_icons=true&theme=transparent&hide_border=true&title_color=3B82F6&icon_color=10B981&include_all_commits=true" alt="GitHub stats" width="48%" />
<img src="https://YOUR-APP.vercel.app/api/top-langs/?username=rahul21896&layout=compact&theme=transparent&hide_border=true&title_color=3B82F6&langs_count=8" alt="Top languages" width="40%" />
```

Because it runs on your own PAT, your instance has no shared rate limit and cannot be paused by someone else.

---

## 4. Spotify Now-Playing (Optional)

A ready-to-use Spotify block is already in the README, **commented out**, just above the "Support" section (search for `novatorem`). To enable it:

1. Create a Spotify app at **developer.spotify.com/dashboard** → note the **Client ID** and **Client Secret**. Add `http://localhost:3000/callback` as a Redirect URI in the app settings.
2. Obtain a **refresh token** with the `user-read-currently-playing` scope (the novatorem README documents the exact authorize-URL → code → token curl flow).
3. Fork `github.com/novatorem/novatorem` and import it at **vercel.com/new** with these environment variables:
   - `SPOTIFY_CLIENT_ID`
   - `SPOTIFY_SECRET_ID` (yes — that repo names the client-secret variable this way)
   - `SPOTIFY_REFRESH_TOKEN`
4. Deploy, then verify `https://YOUR-VERCEL-APP.vercel.app/api/spotify` renders an SVG.
5. In `README.md`, uncomment the block and replace `YOUR-VERCEL-APP` with your Vercel subdomain.

---

## 5. Review These Defaults

Four items were added to fill category sections you didn't explicitly specify. Keep, swap, or extend them — don't leave them unreviewed:

| Section | Default added | Swap ideas |
|---|---|---|
| 🧪 Testing | **PHPUnit** | Pest, Jest, Cypress, Playwright |
| 💡 IDE | **VS Code**, **PHPStorm** | Whatever you actually use daily |
| 🎨 Design | **Figma** | Adobe XD, Sketch — or delete the section if design isn't part of your story |

---

## 6. Skill Icons & Badges

### skillicons.dev

Icon rows are single images with a comma-separated `i=` list:

```html
<img src="https://skillicons.dev/icons?i=php,js,ts,py,html,css,bash" alt="..." />
```

- Add/remove by editing the list. Valid IDs: **skillicons.dev** (full list on the homepage). Common ones used here: `php`, `js`, `ts`, `py`, `laravel`, `react`, `nextjs`, `tailwind`, `aws`, `docker`, `nginx`, `mysql`, `postgres`, `redis`, `elasticsearch`, `wordpress`, `git`, `github`, `githubactions`, `vscode`, `phpstorm`, `figma`.
- Optional params: `&theme=dark|light`, `&perline=N` (wrap after N icons).
- **`apache` and `composer` are NOT valid skillicons IDs** — that's why those two use shields.io badges in the README. If an icon renders as a broken/missing image, the ID is invalid; use a shields badge instead.
- Update the `alt` text when you change the list — it's the accessibility fallback.

### shields.io badge anatomy

```
https://img.shields.io/badge/<LABEL>-<BG_COLOR>?style=for-the-badge&logo=<slug>&logoColor=white
```

- **LABEL**: `_` renders as a space, `--` renders as a literal `-`, `%2B` = `+`, `%2F` = `/`, `%26` = `&`.
- **BG_COLOR**: hex without `#` (use the brand color of the tech, or `3B82F6`/`10B981` for the accent system).
- **style**: this README uses `for-the-badge` for section badges and `flat-square` inside project cards — keep that split for visual hierarchy.
- **logo**: a [Simple Icons](https://simpleicons.org) slug (e.g. `laravel`, `amazonwebservices`, `wordpress`). `logoColor` sets the icon color (`white` or `black` depending on background contrast).

Example — the Composer badge:

```html
<img src="https://img.shields.io/badge/Composer-885630?style=for-the-badge&logo=composer&logoColor=white" alt="Composer" />
```

---

## After Editing

1. Commit to **`master`** (the repo's default branch — not `main`). A push to master also triggers the snake workflow.
2. Confirm repo Settings → Actions → General → Workflow permissions is set to **Read and write permissions**, and that the `METRICS_TOKEN` secret exists (see *GitHub Actions Setup.md*).
3. Hard-refresh your profile page — GitHub caches README images via Camo; changes can take a few minutes to appear.
