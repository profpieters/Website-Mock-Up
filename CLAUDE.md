# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the Dashboard

Serve locally (required for CSV auto-loading):
```bash
python -m http.server 8000
```
Then open http://localhost:8000/dtf_dashboard.html

## Project Overview

Single-page DTF (crypto index) visualization dashboard. No build process or dependencies to install - all libraries loaded via CDN. Light theme inspired by Reserve Protocol's design (cream/off-white background with blue accents).

**Data**: `dtf_prices.csv` contains daily price data for 11 DTF indices (2025-02-26 to 2026-01-08).

## Dashboard Features

### Header
- Title: "DTF Dashboard"
- Subtitle: "Visualizing DTF Indices"
- Disclaimer: "Past performance is not a guarantee of future returns."

### Controls Panel
- **Date Range Slider**: Dual-handle range slider to select date windows (drag handles or drag the range itself)
- **Graph options**:
  - X/Y axis selection with portfolio metrics (Volatility, Annualized Return, Total Return, Sharpe Ratio, Sortino Ratio, Max Drawdown, Calmar Ratio)
  - Comparison dropdown (SP500, DOW, BTC, ETH, USD-EUR, TIPS) - UI only
  - "Must have data" toggle
  - "Size by TVL" toggle - UI only
  - Help icon (?) with metric definitions tooltip
- **Token Characteristic**: Filter checkboxes (All chains, Ethereum, Base, Binance, Yield Token, Index Token) - UI only, no filtering logic yet
- **Performance based on**: Radio buttons (Token Price, Token+Governance Yield, Token+Collateralized Yield) - UI only, no filtering logic yet
- **Performance adjustment**: Checkboxes (None, Rebalancing Cost, Estimated Taxes) with country dropdown and tax disclaimer - UI only, no filtering logic yet

### Tooltips
- Help icon (?) next to "Graph options" shows all metric definitions
- Stat cards have tooltips explaining each metric
- Deep Dive stats table has tooltips on each metric label
- All tooltip icons appear on the left side of labels

### Main Scatter Chart
- Plots DTFs based on selected X/Y metrics
- Click on DTFs to select/deselect for Deep Dive comparison
- "Must have data" toggle - when off, shows DTFs with partial data as hollow circles with completeness percentage
- Stats cards showing highest return, lowest volatility, and best Sharpe ratio (with tooltips)

### Deep Dive Section (Multi-Select Comparison)
- **Click multiple DTFs** in the scatter chart to compare them side-by-side
- Click again to deselect, or use X button on each card, or "Clear All"
- DTFs displayed in responsive grid layout (columns adjust to screen width)
- **Shared controls** at the top apply to all selected DTFs:
  - "Link sliders" toggle - syncs Deep Dive slider with main chart
  - View modes: $10K portfolio (default), Price, % change
- **Each DTF card shows**:
  - About this DTF (placeholder description)
  - Metadata: Market Cap, Created, Minting Fee, Annualized TVL Fee (placeholder values)
  - Time series chart (full data available on zoom, initially zoomed to window)
  - Compact stats table with tooltips: ending value, return, annualized return, volatility, max drawdown, Sharpe/Sortino/Calmar ratios
  - "Detailed View" link to app.reserve.org
- $10K portfolio and % change calculations start from selected window start date

## Color Scheme (Reserve Protocol inspired)
- Background: `#f7f3ef` (cream/off-white)
- Cards/panels: `#fff` (white) with `#e5e5e5` borders
- Accent: `#2775ca` (blue)
- Text: `#1a1a1a` (dark), `#6b7280` (secondary)
- Positive values: `#10b981` (green)
- Negative values: `#ef4444` (red)

## Code Structure

All code is in `dtf_dashboard.html`:
- CSS styles (lines ~11-500)
- HTML structure (lines ~500-710)
- JavaScript (lines ~710-end):
  - `initDashboard()` - parses CSV, initializes main slider
  - `calculateMetrics()` - computes all portfolio metrics for each DTF
  - `updateChart()` - renders the scatter plot with complete/incomplete traces
  - `toggleDtfSelection()` / `removeDtf()` / `clearAllSelections()` - manage selected DTFs
  - `renderAllTimeSeries()` - renders all selected DTF cards
  - `renderSingleTimeSeries()` / `renderSingleStats()` - render individual DTF chart and stats
  - `initTimeseriesSlider()` - manages the Deep Dive slider and linking

## Libraries (CDN)
- Plotly.js - charting
- PapaParse - CSV parsing
- noUiSlider - dual-handle range sliders
