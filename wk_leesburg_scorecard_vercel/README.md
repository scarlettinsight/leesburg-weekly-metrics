# WK Leesburg Weekly Scorecard

Live dashboard — 52 metrics across 4 sources, updated weekly with a one-command push.

## Weekly upload checklist

| File | Toast/SR name | Required? |
|---|---|---|
| `ItemSelectionDetails_*.csv` | PMIX / Items Detail (same thing) | ✅ Required |
| `DiscountDetails_*.csv` | Discount Report | ✅ Required |
| `All_levels.csv` | Menu Item Details | ⚠️ Only if menu prices changed |
| `Sales_by_day.csv` | Sales Summary (daily) | ✅ Recommended |
| `Service_Daypart_summary.csv` | Sales Summary (daypart) | ✅ Recommended |
| `Revenue_center_summary.csv` | Sales Breakdown | ✅ Recommended |
| `Reservations_Export_*.csv` | SevenRooms Reservation Export | ✅ Recommended |

**Note:** Toast "PMIX" and "Items Detail" are the same report. Upload one copy.

## Weekly update workflow

1. Export files above from Toast and SevenRooms for the week
2. Upload to `/mnt/user-data/uploads/`
3. Run the ingest (I do this for you):
   ```
   python weekly_ingest.py --start YYYY-MM-DD --end YYYY-MM-DD
   ```
4. Copy the output into this repo:
   ```
   cp /home/claude/work/runs/scorecard_history.json data/scorecard_history.json
   ```
5. Commit and push:
   ```
   git add data/scorecard_history.json
   git commit -m "Scorecard: week of YYYY-MM-DD"
   git push
   ```
6. Vercel deploys in ~20 seconds. Everyone with the URL sees the update.

## Files

| File | Purpose |
|---|---|
| `index.html` | Dashboard — fetches data fresh on every visit, never needs editing |
| `data/scorecard_baseline.json` | 58-week baseline (Jun 2025–Jun 2026) — commit once, update annually |
| `data/scorecard_history.json` | All ingested weeks — update weekly |
| `vercel.json` | Cache + CORS headers |

## First-time Vercel setup (one-time, ~10 minutes)

1. Push this folder to a GitHub repo (public or private)
2. Go to vercel.com → New Project → Import from GitHub
3. Framework preset: **Other** (it's just static HTML)
4. Root directory: `/` (the default)
5. Click Deploy → get a permanent URL

Share that URL. It never changes.
