# 缠论 (Chanlun) Pine Script v6 Strategy — Implementation Plan

## Overview

This project implements a complete **缠论 (Chan Theory / Entanglement Theory)** trading strategy
in **TradingView Pine Script v6**. The strategy covers the full pipeline from raw candlestick
processing to automated buy/sell signal generation with backtesting support.

---

## Core Chanlun Concepts to Implement

### 1. K-Line Merging / Candle Standardization (K线合并 / 包含处理)

**Purpose:** Eliminate "inclusive" (包含) relationships between adjacent candles to create
a clean, directional price sequence.

**Algorithm:**
- Two adjacent candles have an **inclusive relationship** when one candle's range completely
  contains the other (high1 >= high2 AND low1 <= low2, or vice versa).
- **In an uptrend:** merge by taking the higher high and the higher low.
- **In a downtrend:** merge by taking the lower high and the lower low.
- Direction is determined by comparing the current merged candle to the previous one.
- Process iteratively until no inclusive pairs remain.

**Data structures:** Arrays of merged candle highs/lows/bar indices.

### 2. Fractal Detection (分型识别)

**Purpose:** Identify turning points in the merged candle sequence.

**Types:**
- **Top Fractal (顶分型):** Three consecutive merged candles where the middle one has the
  highest high AND highest low.
- **Bottom Fractal (底分型):** Three consecutive merged candles where the middle one has the
  lowest high AND lowest low.

**Output:** Arrays of fractal types, prices, and bar indices.

### 3. Stroke / Bi Detection (笔的识别)

**Purpose:** Connect alternating top and bottom fractals to form directional strokes.

**Rules:**
- A stroke connects a top fractal to a bottom fractal (downward stroke) or a bottom fractal
  to a top fractal (upward stroke).
- **Minimum distance:** At least 4 merged candles (5 bars including endpoints) between fractals.
- Top fractal's high must be higher than bottom fractal's high.
- Top fractal's low must be higher than bottom fractal's low.
- Strokes must strictly alternate: up-stroke → down-stroke → up-stroke.
- When a new fractal of the same type appears with a more extreme value, extend/replace
  the current endpoint.

**Output:** Arrays of stroke start/end points with prices and bar indices.

### 4. Segment Detection (线段识别)

**Purpose:** Group strokes into higher-level trend segments.

**Algorithm (Characteristic Sequence Method 特征序列法):**
- A segment consists of at least 3 strokes.
- Opposite-direction strokes within a segment form a "characteristic sequence."
- Apply the same inclusion/fractal logic to the characteristic sequence.
- A segment ends when a fractal is confirmed in the characteristic sequence.
- Alternative: **1+1 Termination method** — a segment terminates when a single stroke
  breaks the segment's price channel.

**Output:** Arrays of segment start/end points.

### 5. Pivot / Hub Detection (中枢识别)

**Purpose:** Identify consolidation zones (price ranges where the market oscillates).

**Definition:**
- A pivot is formed by the overlap zone of at least 3 consecutive strokes.
- **Pivot high (ZG):** minimum of the highs of the overlapping strokes.
- **Pivot low (ZD):** maximum of the lows of the overlapping strokes.
- A valid pivot requires ZG > ZD (actual overlap exists).

**Extension rules:**
- A pivot extends when the next stroke stays within the ZG-ZD range.
- A new pivot forms when a stroke breaks out of the current pivot.

**Pivot levels:**
- Level 0 pivot: formed by strokes.
- Higher-level pivots: formed by segments (recursive).

**Output:** Pivot zones with high/low/start/end bar indices.

### 6. Divergence Detection (背驰判断)

**Purpose:** Identify trend exhaustion using MACD momentum comparison.

**Algorithm:**
- Calculate MACD (12, 26, 9) histogram area for each stroke or trend segment.
- **Area calculation:** Sum of absolute MACD histogram values within a stroke/segment.
- **Trend divergence (趋势背驰):** In a trend (same-direction movement with at least
  2 pivots), compare the MACD area of the last segment to the previous same-direction
  segment. If the last area is smaller → divergence.
- **Pivot divergence (盘整背驰):** Within a single pivot, compare the exit stroke's
  MACD area to the entry stroke's area. If smaller → divergence.

**Output:** Boolean flags for divergence at stroke/segment endpoints.

### 7. Buy/Sell Point Identification (买卖点)

**Three types of buy points (买点):**

| Point | Name | Condition |
|-------|------|-----------|
| **B1** | 第一类买点 | End of a downtrend with divergence. Price makes new low but MACD area diminishes. |
| **B2** | 第二类买点 | First pullback after B1 that does NOT make a new low below B1. |
| **B3** | 第三类买点 | Pullback after breaking above a pivot that does NOT re-enter the pivot zone (ZD). |

**Three types of sell points (卖点):**

| Point | Name | Condition |
|-------|------|-----------|
| **S1** | 第一类卖点 | End of an uptrend with divergence. Price makes new high but MACD area diminishes. |
| **S2** | 第二类卖点 | First rally after S1 that does NOT make a new high above S1. |
| **S3** | 第三类卖点 | Rally after breaking below a pivot that does NOT re-enter the pivot zone (ZG). |

---

## Strategy Logic

### Entry Rules:
- **Long entry:** On B1, B2, or B3 signals (configurable which to use).
- **Short entry:** On S1, S2, or S3 signals (configurable which to use).

### Exit Rules:
- **Long exit:** On any sell signal (S1, S2, or S3).
- **Short exit:** On any buy signal (B1, B2, or B3).
- Optional: Fixed stop-loss as percentage below entry.
- Optional: Trailing stop based on pivot levels.

### Position Sizing:
- Configurable: fixed quantity or percentage of equity.

---

## Visual Overlays

| Element | Visualization |
|---------|--------------|
| Merged candles | Optional colored bars |
| Top fractals | Red downward triangle above bar |
| Bottom fractals | Green upward triangle below bar |
| Strokes (Bi) | Solid lines connecting fractals (red=down, green=up) |
| Segments | Thicker lines or different style |
| Pivots | Semi-transparent boxes (yellow/orange fill) |
| Divergence | Labels with "背驰" text |
| Buy points | Green labels (B1/B2/B3) below bars |
| Sell points | Red labels (S1/S2/S3) above bars |

---

## File Structure

```
Chanlun/
├── PLAN.md                          # This plan document
├── README.md                        # Project documentation (Chinese + English)
├── src/
│   └── chanlun_strategy_v6.pine     # Main Pine Script v6 strategy file
└── docs/
    └── chanlun_concepts.md          # Detailed Chanlun theory reference
```

---

## Pine Script v6 Technical Considerations

1. **User-Defined Types (UDT):** Use `type` keyword to define structured types for
   MergedCandle, Fractal, Stroke, Segment, Pivot, and Signal objects.
2. **Arrays & Methods:** Use arrays extensively with methods for managing collections.
3. **Dynamic requests:** Not needed (single timeframe strategy).
4. **Strict type safety:** v6 requires explicit boolean checks (no implicit int→bool cast).
5. **No scope limit:** Allows complex nested logic without worrying about 550-scope cap.
6. **strategy.exit() updates:** Leverage improved absolute/relative parameter handling.
7. **Max bars back:** Use `max_bars_back` annotation where needed for dynamic indexing.
8. **Performance:** Limit historical candle lookback with configurable parameter.

---

## Input Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| Stroke Algorithm | "New" | "Old" / "New" / "4K" stroke rules |
| Min Stroke Bars | 5 | Minimum bars for a valid stroke |
| Show Merged Candles | false | Display merged candle overlay |
| Show Fractals | true | Display fractal markers |
| Show Strokes | true | Display stroke lines |
| Show Segments | false | Display segment lines |
| Show Pivots | true | Display pivot boxes |
| Show Buy/Sell Points | true | Display B1-B3, S1-S3 labels |
| MACD Fast | 12 | MACD fast period |
| MACD Slow | 26 | MACD slow period |
| MACD Signal | 9 | MACD signal period |
| Buy Signals | B1,B2,B3 | Which buy points trigger entries |
| Sell Signals | S1,S2,S3 | Which sell points trigger exits |
| Stop Loss % | 5.0 | Fixed stop loss percentage |
| Use Trailing Stop | false | Enable trailing stop |
| Initial Capital | 100000 | Backtest starting capital |
| Position Size % | 10 | Percentage of equity per trade |
