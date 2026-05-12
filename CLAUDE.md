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
- Four stacked canvas panels: price (flex 6), volume (flex 2), RSI(14) (flex 2), MACD(12,26,9) (flex 2)
- Candlesticks with wicks, green/red (#26a69a / #ef5350)
- EMA 20 (light blue #4fc3f7) and EMA 50 (amber #ffb74d) overlays on price panel
- Current price dashed line with colored badge on right axis
- Crosshair: bright white 2px dashed line (`rgba(255,255,255,0.85)`) synced vertically across all panels
- `mousemove`/`mouseleave` attached to `#chartWrap` so crosshair fires from any panel, not just price
- Horizontal crosshair + right-axis value label shown per panel when hovered:
  - Price panel: price at cursor Y
  - Volume panel: volume formatted K/M/B
  - RSI panel: RSI value to 1 decimal
  - MACD panel: MACD value at cursor Y (symmetric around zero)
- All right-axis cursor labels: solid `#0d0d0d` fill + `#555` border box
- OHLCV tooltip gated to price panel bounds only (hidden when hovering sub-panels)
- Time label box on x-axis: solid `#0d0d0d` fill covering full `PAD.bottom` height — blanks static tick labels underneath; `#555` border
- 24h stats bar: price, % change, high, low
- Auto-refresh every 30s with live countdown in status bar
- Dark terminal aesthetic, monospace font throughout

## Key technical notes
- All timeframe buttons use inline `onclick` in HTML
- Dropdown uses `position: fixed` + `getBoundingClientRect()` to escape toolbar overflow clipping
- Crosshair uses separate overlay canvases (class `.xhair`) per panel with `pointer-events: none`
- X position for crosshair derived from `pricePanel.getBoundingClientRect()` — valid across all panels since they share the same horizontal layout and padding
- Sub-panel Y value at cursor computed inline per mousemove: `maxVol` from candles, RSI scale fixed 0–100, MACD scale via `calcMACD()` abs-max
- Panel element refs cached as `volumePanelEl`, `rsiPanelEl`, `macdPanelEl`; `getBoundingClientRect()` used to detect which panel cursor is in
- `apiFetch()` wrapper reads Binance JSON error body to surface readable messages (e.g. "Invalid symbol.")
- `AbortSignal.timeout` intentionally avoided — not universally supported
