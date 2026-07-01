# HyperStructure 5-in-1 Indicator

A clean, minimalistic Pine Script **v6** suite — `HyperStructure5in1Indicator.pine` — every
module fully toggleable, non-repainting where it matters, and every input has a recommended
("BEST") value in its tooltip.

**Modules:** 1 · Round Numbers · 2 · Moving Averages · 4 · DBox ·
5 · News Danger Zones · 6 · MTF Trend — **5 active**.
Each module has a **single master on/off switch** at the top of its settings group.

---

## Module 1 · Round Number Levels

Horizontal grid at every round price level. Minor lines on the base grid, bolder "major"
lines every Nth level (e.g. the 1000-pip levels). Works on any instrument.

### Settings

| Input | What it does | Best value |
|---|---|---|
| **Show round numbers** | module on/off | on |
| **Spacing mode** | `Pips` (forex) or `Price` (manual non-forex) | Pips for FX |
| **Spacing** | distance between minor levels (forex pips) | 50 |
| **Manual pip** | override auto pip (Pips mode only) | 0 = auto |
| **Auto-fit gold/indices/crypto** | auto round step on non-forex (ignores Spacing) | **on** |
| **Levels each side** | lines above/below price | 10–16 |
| **Major every N** | every Nth line bold | 2 |
| **Minor / Major** | color · width · style (solid/dashed/dotted) | minor dashed, major solid |
| **Labels** | None / Major / All | Major (clean) |
| **Label size / offset** | label sizing & right-shift | Small / 8 |

### Per instrument (with Auto-fit ON — the default)

| Instrument | Handling | Result |
|---|---|---|
| EURUSD, GBPUSD | Pips × your Spacing (50) | 1.3000, 1.3050, 1.3100 … |
| USDJPY | Pips (auto JPY pip) | 145.00, 145.50 … |
| **XAUUSD (gold)** | **auto $50 step** | 2600, 2650, 2700 … (no manual change) |
| XAGUSD (silver) | auto $1 | 30, 31, 32 … |
| US30 / NAS100 | auto 500 | 39000, 39500 … |
| BTCUSD | auto 1000 | 60000, 61000 … |

**Auto-fit** also fixes the **DBox pips** on these symbols: a smart pip (gold = $0.10) means
a $260.66 weekly move shows `~2607 pips · 260.660` instead of the broken `260660`.

**Notes:** levels are anchored to exact round multiples, redraw on the last bar, and span the
full chart width. Turn Auto-fit off if you want full manual control via Spacing mode.

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

## Module 4 · DBox (period range)

Boxes the **full high-low range** of each Day / Week / Month — the whole distance price
travelled in that period — anchored to a chosen timezone (**IST by default**).
**Single master switch:** *Enable DBox*.

### What it draws (per period)

- **Box** from the period **low → high**, filled **light green / red** by the period's
  open vs close (positive period = green, negative = red — updates live).
- **Vertical line** at the period start (full height).

### Auto timeframe

With **Time frame = Auto** (default) the period follows your chart:

| Chart TF | DBox range |
|---|---|
| 15m & below | **Daily** |
| 1H – 2H | **Weekly** |
| 4H & higher | **Monthly** |

Or force **Daily / Weekly / Monthly** to fix it.

### Settings

| Input | What it does | Best value |
|---|---|---|
| **Enable DBox** | master on/off | on |
| **Time frame** | Auto / Daily / Weekly / Monthly | **Auto** |
| **Timezone** | period anchor (midnight/week/month) | Asia/Kolkata (IST) |
| **History (periods)** | how many past boxes to keep | 4–8 |
| **Show vertical line / box** | visibility toggles | as desired |
| **Line / Fill bull / Fill bear** | colors | — |

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
4. Each shown event fills a red **NO TRADE ZONE** box over the **5h before** the event
   (so you don't enter), plus a vertical line at the event and a ⛔ label (currency + note).
   The box renders in the **future** too, so you see the window in advance — and an event is
   **hidden once its window has fully passed** (old news won't clutter the chart).

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

## Module 6 · MTF Trend Dashboard

Top-corner table with a **structured** trend read — each timeframe uses the EMA basis that
fits it — plus a weighted **probability** and a **prediction**.
**Single master switch:** *Enable MTF trend dashboard*.

**Per-TF basis:**

| TF | Basis | Bull when… |
|---|---|---|
| **15m** | Fast EMA (50) | price above a rising 50 |
| **1H / 2H** | Fast + Slow (50/200) | price above **both** and 50 > 200 |
| **4H / Daily** | Slow EMA (200) | price above a rising 200 |

- **Bias %:** higher timeframes weigh more (Daily ×5 … 15m ×1) → how aligned the big picture is.
- **Prediction:** compares short-term (15m/1H/2H) vs higher-TF (4H/Daily) alignment →
  *Uptrend/Downtrend continuation · Pullback in uptrend · Bounce in downtrend · Early shift · Range*.
  A structural read of current alignment — **not a guaranteed forecast**.

| Input | What it does | Best value |
|---|---|---|
| **Enable MTF trend dashboard** | master on/off | on |
| **Fast EMA / Slow EMA** | the two EMAs per the table above | 50 / 200 |
| **Table position** | corner placement | Top Right |
| **Bull / Bear / Neutral** | colors | — |

*All modules active. Combine them with their master switches to taste.*
