# Kanel Sales Hub

Internal sales reporting dashboard for Kanel Inc. Connects directly to Fishbowl API.

## Live URL
**https://adzpatterson.github.io/kanel-sales/**

## Features

- **Fishbowl API Integration** — Pulls sales orders, customers, and product data directly
- **Data Transformation Layer** — Automatically handles:
  - Customer deduplication (Loblaws Inc. / Loblaws Inc merged)
  - Subtotal phantom line item filtering ($1.59M excluded)
  - Shipping & discount line item separation
  - Channel classification (DTC, Grocery, Wholesale, Marketplace, Corporate)
- **24-hour data caching** — Loads instantly after first connection
- **Excel export** on every table

## Reports

### Non-Negotiable (Tier 1)
- Dashboard — KPIs, monthly trend, channel breakdown, top customers
- Revenue by Channel — YoY channel performance with stacked charts
- Revenue by Product — Top 50 products with filtering applied
- Top / Bottom SKUs — Best/worst performers + Pareto concentration chart
- YoY Comparison — Monthly overlay across years

### Critical (Tier 2)
- Wholesale Accounts — Full account list with concentration risk metrics
- Channel Mix Trend — Revenue share shifts over time

### Tools
- Data Quality Monitor — Shows all active transformations, merge rules, and exclusion filters

## Setup

1. Open `index.html` in browser (or host on GitHub Pages)
2. Enter Fishbowl API credentials
3. Data loads in ~15 seconds

## Tech

Single HTML file. No build tools. Uses Chart.js for visualization and SheetJS for Excel export.
