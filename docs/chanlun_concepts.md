# 缠论核心概念 / Chanlun Core Concepts

## 概述 / Overview

缠论 (Chanlun), also known as "The Trading Theory of Entanglement" (缠中说禅), was
developed by a Chinese trading theorist in 2006. It is a complete market analysis
framework based purely on price action, decomposing market movement into a hierarchy
of structural components.

---

## 1. K线包含处理 / K-Line Inclusion Processing

### Definition
When two adjacent candlesticks have an **inclusive relationship** (包含关系), one
candle's price range completely contains the other.

### Conditions
- **Inclusive:** `high1 >= high2 AND low1 <= low2` (or vice versa)
- **Non-inclusive:** Otherwise

### Merge Rules
| Market Direction | Merge Method |
|-----------------|--------------|
| Uptrend (上升) | Take higher high + higher low |
| Downtrend (下降) | Take lower high + lower low |

### Purpose
Eliminates noise and creates a clean, strictly directional sequence of price bars
where each bar is definitively higher or lower than its neighbor.

---

## 2. 分型 / Fractals

### Top Fractal (顶分型)
Three consecutive merged candles where the **middle bar** has:
- The highest HIGH of the three
- The highest LOW of the three

```
    ╱╲
   ╱  ╲     ← Top Fractal (middle is highest)
  ╱    ╲
╱        ╲
```

### Bottom Fractal (底分型)
Three consecutive merged candles where the **middle bar** has:
- The lowest HIGH of the three
- The lowest LOW of the three

```
╲        ╱
  ╲    ╱
   ╲  ╱     ← Bottom Fractal (middle is lowest)
    ╲╱
```

---

## 3. 笔 / Stroke (Bi)

### Definition
A stroke is a directional line segment connecting a **top fractal** to a **bottom
fractal** (or vice versa).

### Rules
1. Must connect a top fractal to a bottom fractal (down stroke) or bottom to top (up stroke)
2. Minimum of **5 merged candles** from endpoint to endpoint (including the fractal centers)
3. Top fractal's high > bottom fractal's high
4. Top fractal's low > bottom fractal's low
5. Strokes must strictly alternate: up → down → up → down

### Types
- **Up Stroke (上升笔):** Bottom fractal → Top fractal (price rising)
- **Down Stroke (下降笔):** Top fractal → Bottom fractal (price falling)

---

## 4. 线段 / Segment

### Definition
A segment is a higher-level structural unit composed of **at least 3 strokes**.

### Detection Methods
1. **Characteristic Sequence Method (特征序列法):**
   - Extract opposite-direction strokes as a sequence
   - Apply inclusion processing to this sequence
   - Look for fractals in the processed sequence
   - A fractal in the characteristic sequence terminates the segment

2. **1+1 Termination (1+1终结法):**
   - A segment terminates when a single stroke completely breaks the segment's channel

---

## 5. 中枢 / Pivot (Zhongshu)

### Definition
A pivot (中枢) is a consolidation zone formed by the **overlapping price range** of
at least 3 consecutive strokes.

### Calculation
```
ZG (pivot high) = min(stroke_1_high, stroke_2_high, stroke_3_high)
ZD (pivot low)  = max(stroke_1_low, stroke_2_low, stroke_3_low)
```

A valid pivot requires: **ZG > ZD** (actual overlap exists)

### Properties
- **Extension:** A pivot extends when subsequent strokes remain within ZG-ZD
- **Break:** A pivot breaks when a stroke exits the ZG-ZD range entirely
- **Level:** Pivots can be recursively defined at higher structural levels

### Significance
- Pivots represent market equilibrium zones
- Trend = movement between pivots
- Consolidation = oscillation within a pivot

---

## 6. 背驰 / Divergence

### Definition
Divergence occurs when price makes a new extreme but **momentum weakens**, signaling
potential trend exhaustion.

### MACD Area Method
- Calculate the sum of absolute MACD histogram values within each stroke/segment
- Compare the MACD area of the current movement to the previous same-direction movement

### Types
| Type | Chinese | Condition |
|------|---------|-----------|
| Trend Divergence | 趋势背驰 | Between pivots: new price extreme with smaller MACD area |
| Pivot Divergence | 盘整背驰 | Within a pivot: exit stroke has smaller MACD area than entry |

---

## 7. 买卖点 / Buy and Sell Points

### Three Buy Points (三类买点)

| # | Name | Chinese | Condition | Strength |
|---|------|---------|-----------|----------|
| B1 | 1st Buy | 第一类买点 | Downtrend ends with bullish divergence | Strongest (trend reversal) |
| B2 | 2nd Buy | 第二类买点 | First pullback after B1 holds above B1 low | Strong (confirmation) |
| B3 | 3rd Buy | 第三类买点 | Pullback after breaking above pivot stays above ZD | Moderate (continuation) |

### Three Sell Points (三类卖点)

| # | Name | Chinese | Condition | Strength |
|---|------|---------|-----------|----------|
| S1 | 1st Sell | 第一类卖点 | Uptrend ends with bearish divergence | Strongest (trend reversal) |
| S2 | 2nd Sell | 第二类卖点 | First rally after S1 fails to exceed S1 high | Strong (confirmation) |
| S3 | 3rd Sell | 第三类卖点 | Rally after breaking below pivot stays below ZG | Moderate (continuation) |

### Trading Logic
```
B1 + B2 = Classic bottom reversal entry
B3     = Trend continuation entry after breakout
S1 + S2 = Classic top reversal exit
S3     = Trend continuation exit after breakdown
```

---

## 8. 走势类型 / Trend Types

| Type | Chinese | Structure |
|------|---------|-----------|
| Uptrend | 上涨走势 | Successive pivots with rising ZG and ZD |
| Downtrend | 下跌走势 | Successive pivots with falling ZG and ZD |
| Consolidation | 盘整走势 | Single pivot with oscillation |

---

## 9. 递归与多级别分析 / Recursion and Multi-Level Analysis

Chanlun is inherently recursive:
- **Level 0:** Strokes formed from candles and fractals
- **Level 1:** Segments formed from strokes
- **Level 2:** Higher segments formed from Level 1 segments
- Each level's structural components become the "candles" for the next level

### Multi-Timeframe Application
- Use higher timeframe pivots for major support/resistance
- Use lower timeframe buy/sell points for precise entry/exit
- Combine signals across 2-3 levels for highest probability trades
