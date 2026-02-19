# 缠论 TradingView 策略 / Chanlun TradingView Strategy

A complete implementation of **缠论 (Chanlun / Chan Theory)** as a Pine Script v6
strategy for TradingView, including automated buy/sell signal detection and backtesting.

## Features

- **K-Line Merging (包含处理)** — Automatic candle standardization with inclusion processing
- **Fractal Detection (分型识别)** — Top and bottom fractal identification
- **Stroke Detection (笔的识别)** — Bi/stroke connection with configurable algorithms
- **Pivot Detection (中枢识别)** — Automatic pivot zone identification and visualization
- **Divergence Detection (背驰判断)** — MACD-based trend exhaustion analysis
- **Buy/Sell Points (买卖点)** — All three types of buy and sell points (B1-B3, S1-S3)
- **Strategy Backtesting** — Full strategy with entries, exits, stop-losses, and trailing stops
- **Visual Overlays** — Fractals, strokes, pivot boxes, divergence labels, and signal markers
- **Info Dashboard** — Real-time stats table showing structure counts and win rate
- **Alert Conditions** — Configurable alerts for all signal types

## File Structure

```
Chanlun/
├── README.md                          # This file
├── PLAN.md                            # Detailed implementation plan
├── src/
│   └── chanlun_strategy_v6.pine       # Main Pine Script v6 strategy
└── docs/
    └── chanlun_concepts.md            # Chanlun theory reference
```

## Usage

1. Open [TradingView](https://www.tradingview.com) Pine Editor
2. Create a new script and select Pine Script v6
3. Copy the contents of `src/chanlun_strategy_v6.pine` into the editor
4. Click "Add to Chart"
5. Open the Strategy Tester tab to see backtest results
6. Adjust parameters in the Settings panel

## Configuration

### Core Parameters
| Parameter | Default | Description |
|-----------|---------|-------------|
| Min Stroke Bars | 5 | Minimum candles for a valid stroke |
| Stroke Algorithm | New | Stroke detection algorithm |

### Strategy Settings
| Parameter | Default | Description |
|-----------|---------|-------------|
| Use B1/B2/B3 | All enabled | Which buy points trigger long entries |
| Use S1/S2/S3 | All enabled | Which sell points trigger short entries |
| Stop Loss % | 5.0 | Fixed stop-loss percentage |
| Trailing Stop | Disabled | Enable trailing stop |
| Long Only | Disabled | Disable short entries |

### Visual Settings
Toggle visibility of merged candles, fractals, strokes, pivots, buy/sell points,
and divergence labels independently.

## Signal Types

| Signal | Name | Description |
|--------|------|-------------|
| B1 (一买) | 1st Buy Point | Trend reversal with bullish divergence |
| B2 (二买) | 2nd Buy Point | Pullback confirmation after B1 |
| B3 (三买) | 3rd Buy Point | Continuation after pivot breakout |
| S1 (一卖) | 1st Sell Point | Trend reversal with bearish divergence |
| S2 (二卖) | 2nd Sell Point | Rally confirmation after S1 |
| S3 (三卖) | 3rd Sell Point | Continuation after pivot breakdown |

## Theory Reference

See [docs/chanlun_concepts.md](docs/chanlun_concepts.md) for a comprehensive guide
to all Chanlun concepts implemented in this strategy.

## Requirements

- TradingView account (free or paid)
- Pine Script v6 compatible chart

## License

Mozilla Public License 2.0
