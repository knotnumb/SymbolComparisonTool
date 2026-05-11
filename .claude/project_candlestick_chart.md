---
name: Candlestick Chart Project
description: Single-file HTML candlestick chart tool using Binance public API
type: project
originSessionId: 0abb2b0a-089b-4371-ba26-f3bdd37795d6
---
./index.html

**Why:** Built from scratch as a dark-themed trading terminal in vanilla JS with no external libraries.

**How to apply:** When the user references the chart tool or wants to continue development, read that file first to get current state.

## Features implemented
- Binance public REST API (`https://api.binance.com/api/v3`)
- Searchable symbol dropdown (fetches full symbol list from `ticker/price`)
- Timeframe buttons: 5m, 15m, 1h, 4h, 1D
- Three stacked canvas panels: price (60%), volume (20%), RSI(14) (20%)
- Candlesticks with wicks, green/red (#26a69a / #ef5350)
- EMA 20 (light blue #4fc3f7) and EMA 50 (amber #ffb74d) overlays
- Current price dashed line with colored badge on right axis
- Crosshair with OHLCV tooltip on hover, synced vertical line across all panels
- 24h stats bar: price, % change, high, low
- Auto-refresh every 30s with live countdown in status bar
- Dark terminal aesthetic, monospace font throughout

## Key technical notes
- All event wiring uses inline `onclick`/`onkeyup` in HTML to avoid JS listener ordering bugs
- Dropdown uses `position: fixed` + `getBoundingClientRect()` to escape toolbar overflow clipping
- Crosshair uses separate overlay canvases (class `.xhair`) per panel with `pointer-events: none`
- `apiFetch()` wrapper reads Binance JSON error body to surface readable messages (e.g. "Invalid symbol.")
- `AbortSignal.timeout` was intentionally avoided — not universally supported
