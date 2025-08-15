
# 🌕 Lunar Phase Crypto Backtester

**Lunar Phase Crypto Backtester** is a backtesting tool that analyzes the relationship between lunar phases (New Moon & Full Moon) and crypto asset returns over a given time window.

flowchart TD
    A[Start] --> B[Read CLI Parameters<br/>(pair, window, threshold,<br/>start/end, bins/range, prefix,<br/>export-raw, no-pdf, phase-window,<br/>regime, MA)]
    B --> C[Download Historical Prices<br/>(Yahoo Finance)]
    C --> D[Compute Lunar Events<br/>(New Moon & Full Moon via ephem)]
    D --> E[Expand Event Dates by ±W Days<br/>(--phase-window)]
    E --> F[Align to Trading Calendar (UTC)<br/>De-duplicate Starts]
    F --> G[Apply Regime Filter (--regime)<br/>Close vs MA(--ma)]
    G --> H[Build Start Lists:<br/>New Moon / Full Moon / Base]
    H --> I[For Each Start:<br/>Pick Start Close & End Close<br/>(+N days, aligned)]
    I --> J[Compute Returns:<br/>(End-Start)/Start]
    J --> K1[Aggregate Statistics<br/>(HitRate_Up, MoveUp>=X%,<br/>MoveDown<=-X%, Avg, Median)]
    J --> K2[Plot Histograms:<br/>New / Full / Base]
    K1 --> L1[Export CSV Summary<br/>(<prefix>_stats.csv)]
    K2 --> L2[Export PNG Charts<br/>(_hist_new/_full/_base/_compare)]
    J --> L3{--export-raw?}
    L3 -->|Yes| L3a[Export Raw Returns CSV<br/>(_returns_new/_full/_base)]
    L3 -->|No| L3b[Skip Raw Export]
    K1 --> M{--no-pdf?}
    M -->|No| M1[Generate PDF Report<br/>(table + charts + narrative)]
    M -->|Yes| M2[Skip PDF]
    L1 --> Z[End]
    L2 --> Z
    L3a --> Z
    M1 --> Z


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
git clone https://github.com/yourusername/lunar-phase-crypto-backtester.git
cd lunar-phase-crypto-backtester
