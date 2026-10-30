# Country News Desk

A free news channel per country. Stories are ranked by coverage, source credibility, recency, global reach and evidence. Each story has a Verified / Partly confirmed / Unverified badge and a short cartoon explainer. Saving or dismissing stories teaches the ranking what you find important (stored in your browser).

**How it runs:** a GitHub Actions job fetches Google News RSS every 6 hours (free, no API key) and commits `docs/data/news.json`. The static site in `docs/` is served by GitHub Pages or Render. Until the first update runs, the site shows sample data with invented headlines.

## Files
| Path | Purpose |
|---|---|
| `docs/index.html` | The dashboard (ranking, evidence, cartoons, saved archive) |
| `docs/data/news.json` | Live data, rewritten by the workflow |
| `docs/data/sample.json` | Demo data shown before the first update |
| `fetch_news.py` | Fetches, groups and scores stories (Python standard library only) |
| `.github/workflows/news.yml` | Runs the fetch every 6 hours and commits the result |
| `render.yaml` | Render blueprint for the static site |

## 1. Put it on GitHub
1. Create a free account at github.com and a new **public** repository.
2. Upload everything from this folder, including the hidden `.github` folder (drag the whole folder contents into "Add file > Upload files").
3. Open the **Actions** tab, enable workflows if asked, choose **Update news**, and click **Run workflow**. Wait about a minute for the green tick.

## 2a. Host on GitHub Pages (simplest)
Settings > Pages > Source: "Deploy from a branch" > Branch `main`, folder `/docs` > Save. Your site is at `https://<username>.github.io/<repo>/`.

## 2b. Or host on Render (free static site)
1. Sign in at render.com with GitHub.
2. New > **Blueprint**, pick this repository (it reads `render.yaml`).
   Manual alternative: New > Static Site, publish directory `docs`, no build command.
3. Every time the workflow commits new data, Render redeploys automatically.

Render's cron jobs are paid, so the free setup keeps the fetching on GitHub Actions and only uses Render for hosting.

## Add countries
Add a line to `COUNTRIES` in `fetch_news.py`: `"BR": ("Brazil", "🇧🇷", "pt-BR", "BR", "BR:pt-419")`. Non-English editions fetch fine, but topic detection only knows English keywords.

## Known limits
- Photo and location evidence are empty until a step is added that reads each article's image and place name.
- Cartoons are drawn animations matched to the topic, not generated video.
- Google News RSS is an unofficial feed. Keep usage modest and link back to sources.
- GitHub pauses scheduled workflows after about 60 days of no repository activity. If updates stop, run the workflow once to wake it.
- This code was tested with simulated feeds only. If the first Actions run fails, open its log and fix the error shown.
