# HyperStructure 5-in-1 Indicator

A clean, minimalistic Pine Script **v6** suite — `HyperStructure5in1Indicator.pine` — built as
up to 5 modules added one at a time. Every module is fully toggleable, non-repainting where it
matters, and every input has a recommended ("BEST") value in its tooltip.

**Modules:** 1 · Round Numbers · 2 · Moving Averages · 3 · Validation Candles · 4 · DBox · 5 · News Danger Zones — **all 5 active**.
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

## Module 4 · DBox (period range)

Boxes the **full high-low range** of each Day / Week / Month — the whole distance price
travelled in that period — anchored to a chosen timezone (**IST by default**).
**Single master switch:** *Enable DBox*.

### What it draws (per period)

- **Box** from the period **low → high**, filled **light green / red** by the period's
  open vs close (positive period = green, negative = red — updates live).
- **Vertical line** at the period start (full height).
- **Price-movement label** — pips + price difference + ▲/▼ — on **every box**
  (e.g. `320 pips · 0.03200 ▲`). Each completed box keeps its final value.

### Settings

| Input | What it does | Best value |
|---|---|---|
| **Enable DBox** | master on/off | on |
| **Time frame** | Daily / Weekly / Monthly | Daily |
| **Timezone** | period anchor (midnight/week/month) | Asia/Kolkata (IST) |
| **History (periods)** | how many past boxes to keep | 4–8 |
| **Show vertical line / box / price movement** | visibility toggles | as desired |
| **Line / Fill bull / Fill bear / Positive / Negative** | colors | — |

**Notes:** periods start at the chosen timezone's boundary (IST midnight for Daily). The
current box grows as each bar **closes** (no intrabar flicker); past boxes stay fixed.

---

## Module 5 · News Danger Zones

Red **no-trade zone** over the hours before each high-impact ("red folder") event.
**Single master switch:** *Enable news danger zones*.

> **Important — read this:** Pine Script has **no access to an economic calendar**. It
> cannot auto-detect red-folder news. So you **paste the event times** (a ~1-minute weekly
> copy from ForexFactory). Every TradingView news indicator works this way.

### How to use (keep ONE master list for all charts)

1. On ForexFactory: filter to **high-impact (red)** only, and set its **timezone to match**
   the **News timezone** input (default IST). *(This is critical — wrong timezone = wrong zone.)*
2. Paste each red event as `YYYY-MM-DD HH:MM CUR note`, comma-separated. Time can be 24h
   or am/pm. **Include the 3-letter currency** — that's what powers auto-filter:
   `2026-06-22 18:00 CAD CPI, 2026-06-25 6:00pm USD PCE`
3. Leave **Only this chart's currencies** ON. Now EURUSD shows only EUR/USD events, USDCAD
   shows USD/CAD, gold (XAUUSD) shows USD — **from the same list**. No per-chart editing.
4. Each shown event fills a red **RESTRICTED AREA** box over the **5h before** the event
   (so you don't enter), plus a vertical line at the event and a ⛔ label (currency + note).
   The box renders in the **future** too, so you see the restricted window in advance.

> You only ever transcribe the red events once per week; the script picks the relevant ones
> per symbol automatically. Same list works on every chart.
>
> **Filtering needs the currency code.** An entry written without its 3-letter currency
> (e.g. `2026-06-22 18:00 CPI`) can't be filtered and will show on every chart. Always
> include `CUR` (e.g. `... 18:00 USD CPI`).

### Settings

| Input | What it does | Best value |
|---|---|---|
| **Enable news danger zones** | master on/off | on |
| **Red-folder news** | one master event list (with currency codes) | update weekly |
| **Only this chart's currencies** | auto-filter events to the symbol's pair | **on** |
| **News timezone** | timezone of the entered times (match ForexFactory) | Asia/Kolkata (IST) |
| **Hours before** | no-trade window length before event | **5** |
| **Hours after** | optional post-event window | 0 (0.5–1 for spike) |
| **Danger color / Zone transparency** | fill styling | red / 85 |

**Notes:** zones and lines render in the **future** too, so upcoming events warn you ahead of
time. The background fill appears on bars inside the window; the lines + ⚠ label mark the
event even before price gets there. Non-repainting.

---

*All 5 modules complete. Combine modules with their master switches to taste.*
