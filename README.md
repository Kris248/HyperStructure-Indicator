# HyperStructure 5-in-1 Indicator

A clean, minimalistic Pine Script **v6** suite — `HyperStructure5in1Indicator.pine` — built as
up to 5 modules added one at a time. Every module is fully toggleable, non-repainting where it
matters, and every input has a recommended ("BEST") value in its tooltip.

**Modules:** 1 · Round Numbers · 2 · Moving Averages · 3 · Validation Candles (active) — 4–5 coming.
Each module has a **single master on/off switch** at the top of its settings group.

---

## Module 1 · Round Number Levels

Horizontal grid at every round price level. Minor lines on the base grid, bolder "major"
lines every Nth level (e.g. the 1000-pip levels). Works on any instrument.

### Settings

| Input | What it does | Best value |
|---|---|---|
| **Show round numbers** | module on/off | on |
| **Spacing mode** | `Pips` (forex) or `Price` (gold/indices/crypto) | Pips for FX |
| **Spacing** | distance between minor levels | 500 or 1000 (FX) |
| **Manual pip** | override auto pip (Pips mode only) | 0 = auto |
| **Levels each side** | lines above/below price | 10–16 |
| **Major every N** | every Nth line bold | 2 |
| **Minor / Major** | color · width · style (solid/dashed/dotted) | minor dashed, major solid |
| **Labels** | None / Major / All | Major (clean) |
| **Label size / offset** | label sizing & right-shift | Small / 8 |

### How to set it per instrument

| Instrument | Mode | Spacing | Result |
|---|---|---|---|
| EURUSD, GBPUSD | Pips | 500 | 1.3000, 1.3050, 1.3100 … (1000-levels bold) |
| EURUSD (wider) | Pips | 1000 | 1.3000, 1.3100, 1.3200 … |
| USDJPY | Pips | 500 | 145.00, 145.50 … (auto JPY pip) |
| XAUUSD (gold) | Price | 10 | 2000, 2010, 2020 … |
| US30 / NAS100 | Price | 100 | 39000, 39100 … |

**Notes:** levels are anchored to exact round multiples, redraw on the last bar (no
clutter build-up), and span the full chart width.

---

## Module 2 · Moving Averages

Full MA suite — up to 3 lines, **12 selectable types**, fill cloud and value labels.
**Single master switch:** *Enable Moving Averages*.

### MA types (concept)

| Type | Concept | Use |
|---|---|---|
| **EMA** | exponential, most-watched | **best for swing** — acts as real dynamic S/R |
| SMA | simple average | smoothest, slowest |
| WMA | linearly weighted | faster than SMA |
| RMA | Wilder's smoothing | very smooth |
| **HMA** | Hull | **best low-lag** — fast + smooth |
| ZLEMA / DEMA / TEMA | lag-reduced EMAs | fast, more whippy |
| ALMA | Gaussian window | tune offset/sigma |
| VWMA / VWZL / VHMA | volume-weighted | need **real volume** (futures, indices, gold, crypto) |

### Settings

| Input | What it does | Best value |
|---|---|---|
| **Enable Moving Averages** | master on/off | on |
| **MA type** | one type applied to all 3 lines | EMA (or HMA for low-lag) |
| **Source / Line style** | price input · Line/Stepline/Circles | close / Line |
| **MA1 / MA2 / MA3** | toggle · length · width · color | **21 / 50 / 200** |
| **Fill MA1/MA2** | green/red momentum cloud | on |
| **Value labels** | price tags at the right edge | on |

**Swing best-setup:** EMA, lengths 21 / 50 / 200 — trigger / swing-trend / macro filter.
For low-lag entries switch the type to **HMA** or **VWZL**.

---

## Module 3 · Validation Candles

A market-structure chain. A **reference candle's** high and low are the levels to beat.
**Single master switch:** *Enable validation candles*.

### Logic

1. The reference candle's **high / low** define the active range.
2. Candles that stay **inside** that range are **invalidated** (greyed).
3. When a later candle **closes beyond** the high (bull) or low (bear) with a **solid body**
   (a real breakout, not a wick), it is **validated** and becomes the **new reference**.
4. The reference is **stored until broken** — the breakout can be the next candle or land
   `n` candles later, after `n` inside/invalidated candles. Either way the breaker is colored.
5. A **live box** marks the active reference and updates **only after a candle closes**
   (never intrabar → no repaint, no flicker).

### Settings

| Input | What it does | Best value |
|---|---|---|
| **Enable validation candles** | master on/off | on |
| **Solid breakout: min body** | how solid the breaking candle must be (body ÷ range) | 0.6 (strict 0.7) |
| **Break buffer (× ATR)** | optional anti-fakeout margin past the level | 0 (0.10 on noisy pairs) |
| **Color invalidated candles** | grey the inside candles | on |
| **Live reference box** | box on the active reference, updates on close | on |
| **Colors** | validated bull / bear / invalidated / box | — |

**Colors:** validated bull = teal, validated bear = red, invalidated = grey, reference box = amber.
Non-repainting: the chain and box commit on candle **close** only.

---

*Next up: add Module 4 into `HyperStructure5in1Indicator.pine`.*
