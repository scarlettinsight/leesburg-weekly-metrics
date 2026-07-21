# WK Leesburg Weekly Scorecard

Live dashboard for The Wine Kitchen – Leesburg. Updates every week with a one-command push.

## Weekly update workflow

1. Export from Toast: `ItemSelectionDetails` + `DiscountDetails` for the week
2. Drop into `/mnt/user-data/uploads/` alongside `All_levels.csv`
3. Run the ingest:
   ```bash
   python weekly_ingest.py --start YYYY-MM-DD --end YYYY-MM-DD
   ```
4. Copy the output into this repo:
   ```bash
   cp /home/claude/work/runs/scorecard_history.json data/scorecard_history.json
   ```
5. Commit and push:
   ```bash
   git add data/scorecard_history.json
   git commit -m "Scorecard: week of YYYY-MM-DD"
   git push
   ```
6. Vercel auto-deploys in ~20 seconds. Done.

## Files

| File | Purpose |
|---|---|
| `index.html` | The dashboard — fetches data on load, never needs editing |
| `data/scorecard_baseline.json` | 58-week baseline (Jun 2025–Jun 2026) — commit once |
| `data/scorecard_history.json` | All ingested weeks — updated weekly |
| `vercel.json` | Cache and CORS headers for the data files |

## First-time Vercel setup

1. Push this folder to a GitHub repo (public or private)
2. Go to [vercel.com](https://vercel.com) → New Project → Import from GitHub
3. Framework preset: **Other** (it's just static HTML)
4. Root directory: `/` (the default)
5. Click Deploy — done

Vercel gives you a permanent URL like `wk-leesburg-scorecard.vercel.app`.
Share that URL with anyone who needs weekly access.

## Baseline refresh

The `scorecard_baseline.json` only needs to be rebuilt if you want to update
the comparison baseline (e.g. after a full year of new data). Run:
```bash
python wk_leesburg_analysis.py   # full pipeline
# then copy the result:
cp /home/claude/work/runs/scorecard_baseline.json data/scorecard_baseline.json
```
