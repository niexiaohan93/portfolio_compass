# Portfolio Compass

A single-file, browser-based tool for evaluating a stock portfolio's risk and expected return — and checking whether it actually behaves like a market portfolio or like an active, concentrated bet.

No build step, no backend, no dependencies to install. Open `portfolio-compass.html` in a browser and it works.

> Educational calculator, not investment advice. All figures are estimates derived entirely from the data you provide — the tool has no connection to any live market data source.

## What it does

You provide a benchmark index and a list of holdings (via pasted historical prices, CSV upload, or Excel upload). From that, the tool derives:

- **Per-holding metrics** — annualized volatility, beta (vs. your benchmark), historical return, and a CAPM-based expected return (`rf + β × (market return − rf)`)
- **Portfolio-level metrics** — expected return, volatility, beta, Sharpe ratio, R² vs. the benchmark, and concentration (Herfindahl-Hirschman Index)
- **A plain-language verdict** on whether the portfolio behaves like a market portfolio (beta ≈ 1, well-diversified, high R² against the benchmark) or like a concentrated, active bet
- **Visuals** — a beta gauge, a security market line (beta vs. expected return), a risk/return scatter plot, and a sector-mix comparison against an editable reference

## Getting started

1. Download `portfolio-compass.html`
2. Open it in any modern browser (double-click, or `open portfolio-compass.html`)
3. That's it — everything runs client-side in the page

No installation, no server, no API keys required for the core tool.

## Using it

### 1. Market & risk-free rate
Enter your benchmark's name, a risk-free rate (e.g. a local 10-year government bond yield), and the benchmark's historical prices (paste, or upload a CSV/Excel file). Results here — historical return, volatility, and the effective market return used in CAPM — update live as soon as there's enough data, even before you add any holdings.

An optional "expected market return override" lets you substitute a forward-looking estimate instead of the historical average.

### 2. Holdings
Add each holding with a name, ticker, sector (chosen from a dropdown kept in sync with your sector reference list), and weight. For each one, either:
- **Paste or upload** historical prices (same date range/frequency as the benchmark) — volatility, beta, and CAPM expected return are calculated automatically, or
- **Switch to manual estimate** and type in a return/vol/beta directly, if you don't have price history for that name

A "Load 3 sample holdings" button fills in synthetic demo data so you can see the tool working before entering your own numbers. "Reset all data" (available in both Section 1 and Section 2) clears everything back to a blank state.

### 3. Portfolio results
Aggregate expected return, volatility, Sharpe ratio, beta, R², and concentration, plus the market-portfolio verdict with the reasoning behind it.

### 4. Visuals
Beta gauge, SML chart, risk/return scatter, and sector-mix bar chart (compared against an editable reference sector mix — edit it to match your actual benchmark's published sector weights for a meaningful comparison).

## Supported data formats

- **Paste**: comma or newline-separated prices, oldest → newest
- **CSV**: auto-detects a `close`/`price`/收盘 column (plus a date column) if there's a header row, or falls back to a single column of prices with no header
- **Excel (.xlsx/.xls)**: reads the first sheet using the same column-detection logic. Also gracefully handles files that are actually a CSV or an HTML table saved with an Excel file extension (common with "export to Excel" buttons on some finance sites)

## Methodology notes

- **Beta and volatility** are computed from the return series you provide (`(pₜ − pₜ₋₁) / pₜ₋₁`), not fetched from anywhere.
- **Expected return** defaults to CAPM; you can override it per holding via manual mode.
- **Portfolio variance** uses Sharpe's single-index model — combining each holding's own variance with beta-implied co-movement — rather than a full pairwise covariance matrix. This is a standard simplification for a lightweight tool, not a substitute for a full covariance-matrix approach.
- **The market-portfolio verdict** combines three checks: beta close to 1, low concentration (HHI), and high R² against the benchmark.
- **"Is this a market portfolio?" is a different question from "is this on the efficient frontier?"** The former checks how closely a portfolio's behavior mirrors the market; the latter checks whether, given a portfolio's own assumptions, no better risk/return combination exists among those same holdings. This tool only answers the first question.

## Tech

Single HTML file — vanilla JavaScript, no framework. Uses:
- [Chart.js](https://www.chartjs.org/) for the SML, risk/return, and sector charts (loaded with automatic CDN fallback)
- [SheetJS](https://sheetjs.com/) for Excel parsing (same fallback approach)
- Hand-drawn SVG for the beta gauge

Both libraries are loaded dynamically at runtime with fallbacks across multiple CDNs; if all of them are unreachable, the beta gauge and calculations still work, but the three Chart.js visuals will show a banner explaining why they're blank.

## Limitations

- All figures are only as good as the data you enter — the tool doesn't validate that a benchmark name matches the prices you actually pasted.
- No live data fetching. You're responsible for sourcing historical prices yourself (your brokerage, Yahoo Finance, Google Finance/Sheets, Investing.com, Stooq, or a regional provider).
- Portfolio variance uses the single-index model simplification described above, not a full covariance matrix.
- This is an educational tool, not financial advice.

## License

MIT.
