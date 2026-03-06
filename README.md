# Kanel Sales Hub
Internal sales reporting dashboard for Kanel Inc. Powered by the Kanel Operations Dashboard Google Sheet as a nightly data lake.

## Live URL
**https://adzpatterson.github.io/kanel-sales/**

## Architecture
No local proxy, no Node.js, no credentials in the browser. Data flows as follows:

```
Fishbowl API
     |
Cloudflare Worker (kanel-fishbowl-proxy.kanelhub.workers.dev)
     |
Google Apps Script - Nightly Sync (2 AM)
     |
Kanel Operations Dashboard (Google Sheet, 22 tabs)
     |
buildSalesHubCache() - runs after sync
     |
_SalesHubCache tab (JSON blob)
     |
Apps Script Web App (doGet) - serves JSON
     |
index.html (GitHub Pages)
```

## Features
- **No-auth frontend** -- static HTML fetches from Apps Script Web App, no login required
- **Nightly refresh** -- cache rebuilds automatically at 2 AM alongside the full Fishbowl sync
- **Cost enrichment** -- 7-tier resolved costs from `_ProductCosts` tab applied to all margin calculations
- **Data transformation** -- customer deduplication, subtotal/shipping/discount filtering, channel classification
- **Excel export** on every table

## Reports
### Command Center
- **Executive Summary** -- YTD revenue, orders, AOV, MTD, WTD, channel and category donuts
- **KPI Dashboard** -- YTD/MTD/WTD KPIs, AOV by channel, top 20 customers, category breakdown
- **Weekly Pulse** -- TW vs LW comparison, daily trend, top customers this week

### Revenue Intelligence
- **Channel Deep Dive** -- YTD channel mix, top 30 customers by channel
- **Category Mix** -- Revenue by product category with donut chart
- **SKU Performance** -- Top 50 products YTD with monthly trend
- **Monthly Trend** -- Rolling 12-month bar chart, momentum (last 3 vs prior 3 months)

### Profitability
- **Margin Analysis** -- Gross margin by SKU, channel, and month using resolved costs
- **Wholesale Accounts** -- Full account list with concentration risk metrics

## Apps Script Files
Two files in the same Apps Script project attached to the Kanel Operations Dashboard sheet:

| File | Purpose |
|------|---------|
| `kanel_fishbowl_nightly_sync.gs` | Writes 22 Fishbowl data tabs nightly at 2 AM |
| `kanel_sales_hub_cache.gs` | Reads those tabs, crunches aggregations, writes `_SalesHubCache` tab. Also exposes `doGet()` as the Web App endpoint. |

## Setup (new instance)
1. Open the Kanel Operations Dashboard in Google Sheets
2. Go to Extensions > Apps Script
3. Paste `kanel_fishbowl_nightly_sync.gs` into the sync file, `kanel_sales_hub_cache.gs` as a second file
4. Set Script Properties: `PROXY_URL`, `PROXY_SECRET`, `FB_PASSWORD`
5. Paste `product_costs_v3.csv` into a sheet tab named `_ProductCosts`
6. Run `runFullSyncNow()` to populate all 22 tabs
7. Run `buildSalesHubCache()` to build the JSON cache
8. Deploy as Web App (Execute as: Me, Anyone can access), copy the `/exec` URL
9. Paste the URL into `index.html` line 134 as `CACHE_URL`
10. Push `index.html` to GitHub Pages

## Nightly Trigger
The Apps Script trigger runs `runFullSyncNow()` at 2 AM daily. To also rebuild the Sales Hub cache automatically, change the trigger target to `runFullSyncWithCache()`.

## Tech
Single HTML file. No build tools. Uses vanilla JS for all rendering.
