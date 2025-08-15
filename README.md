# 🌕 Lunar Phase Crypto Backtester

**Lunar Phase Crypto Backtester** is a backtesting tool that analyzes the relationship between lunar phases (New Moon & Full Moon) and crypto asset returns over a given time window.

It uses historical price data from Yahoo Finance and lunar phase data from the `ephem` library, and produces:
- **Statistical summary table** for each phase.
- **Return distribution histograms** for New Moon, Full Moon, and baseline periods.
- **Overlay histogram comparison**.
- **PDF report** with **auto-generated analysis narrative**.
- *(Optional)* Raw CSV export for detailed data analysis.

---

## ✨ Features

- **Multi-asset support** – works with any ticker available on Yahoo Finance (`--pair BTC-USD`, `ETH-USD`, etc.).
- **Flexible parameters**:
  - `--threshold`: Minimum move to count as significant.
  - `--window`: Return calculation window (days).
  - `--start` / `--end`: Custom backtest date range.
  - `--bins` & `--range`: Histogram customization.
- **Export raw data** – save per-category returns to CSV (`--export-raw`).
- **Professional PDF report** – includes table, charts, and commentary.
- **CLI only** – no code edits required.

---

## 📦 Installation

### 1. Clone the repository
```bash
git clone https://github.com/irfanfiles/lunar-phase-crypto-backtester.git
cd lunar-phase-crypto-backtester
```

---

## 📊 Process Flowchart

```mermaid
flowchart TD
    A[Start] --> B["Read CLI Parameters
    (pair, window, threshold,
    start/end, bins/range, prefix,
    export-raw, no-pdf, phase-window,
    regime, MA)"]
    B --> C["Download Historical Prices
    (Yahoo Finance)"]
    C --> D["Compute Lunar Events
    (New Moon & Full Moon via ephem)"]
    D --> E["Expand Event Dates by ±W Days
    (--phase-window)"]
    E --> F["Align to Trading Calendar (UTC)
    De-duplicate Starts"]
    F --> G["Apply Regime Filter (--regime)
    Close vs MA(--ma)"]
    G --> H["Build Start Lists:
    New Moon / Full Moon / Base"]
    H --> I["For Each Start:
    Pick Start Close & End Close
    (+N days, aligned)"]
    I --> J["Compute Returns:
    (End-Start)/Start"]
    J --> K1["Aggregate Statistics
    (HitRate_Up, MoveUp>=X%,
    MoveDown<=-X%, Avg, Median)"]
    J --> K2["Plot Histograms:
    New / Full / Base"]
    K1 --> L1["Export CSV Summary
    (<prefix>_stats.csv)"]
    K2 --> L2["Export PNG Charts
    (_hist_new/_full/_base/_compare)"]
    J --> L3{--export-raw?}
    L3 -->|Yes| L3a["Export Raw Returns CSV
    (_returns_new/_full/_base)"]
    L3 -->|No| L3b[Skip Raw Export]
    K1 --> M{--no-pdf?}
    M -->|No| M1["Generate PDF Report
    (table + charts + narrative)"]
    M -->|Yes| M2[Skip PDF]
    L1 --> Z[End]
    L2 --> Z
    L3a --> Z
    L3b --> Z
    M1 --> Z
    M2 --> Z
```
