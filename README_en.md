# Stock Analyze Prompts

<div align="center">

[简体中文](README.md) | [English](README_en.md) | [Français](README_fr.md)

</div>

---

A collection of AI prompts for stock analysis, providing two systematic value-investing frameworks designed for use with Claude and other LLMs. Strategy concepts inspired by the Bilibili creator "史诗级韭菜".

## Framework 1: Cigar Butt Prompt (v1.8)

A quantitative analysis framework for undervalued Hong Kong and A-share stocks. Built on Graham-style net asset discount logic, it systematically identifies three cigar-butt sub-types, runs a 22-item Fact Check, and outputs a tiered safety-margin rating with half-Kelly position sizing.

**Three Sub-types Covered:**

| Sub-type | Core Logic |
|---|---|
| A — Asset Discount | Net cash / asset discount; market cap below liquidation value |
| B — Subsidiary Unlock | Listed subsidiary market cap exceeds parent's total market cap |
| C — Buyback / Privatization | Ongoing buybacks or privatization expectation with clear value-release path |

**How to Use:**

1. Paste the full content of [`cigbutt/烟蒂股prompt_v1.8.md`](cigbutt/烟蒂股prompt_v1.8.md) as the System Prompt in Claude (or another LLM)
2. Provide the target: stock code or company name, plus at least 2 periods of financial reports (PDF upload or pasted table)
3. The AI fetches live market cap, stock price, dividend yield, etc. via yfinance MCP / WebSearch and generates a complete Markdown analysis report

**Example Input:**

```
Analyze 3328.HK Bank of Communications. I have uploaded the 2023 Annual Report and 2024 Interim Report PDFs.
```

**Output Includes:**
- Asset cushion (T-tier safety margin) calculation
- Sub-type determination with every core condition checked
- 22-item Fact Check (Data-type vs. Risk-type warning classification)
- Overall rating (A / B / C / Rejected)
- Half-Kelly position sizing recommendation

**Accounting Standards:** HKFRS / IFRS, CN-GAAP

---

## Framework 2: Turtle Strategy (v0.15)

A systematic multi-agent investment analysis framework covering HK, A-share, and US markets. Uses a Coordinator + three-phase Sub-agent architecture to generate in-depth investment analysis reports for any listed company.

**Analysis Pipeline:**

```
User Input (stock code + optional annual report PDF)
        ↓
  Coordinator — parses input, schedules all phases
        ↓
  Phase 1 — Data Collection
  (yfinance MCP: market data, financial statements, dividend history)
        ↓
  Phase 2 — PDF Parsing
  (structured extraction from annual / interim reports)
        ↓
  Phase 3 — Analysis & Report
  (valuation, competitive moat, risk assessment, half-Kelly sizing)
```

**How to Use (Claude Projects / API):**

1. Configure yfinance MCP (or equivalent stock data MCP tool)
2. Set [`coordinator.md`](turtle_framework/乌龟战法_v0.15/coordinator.md) as the System Prompt for the main Agent
3. Configure Phase 1–3 as System Prompts for the respective Sub-agents
4. Input a stock code in the main conversation (optionally attach an annual report PDF)

**Example Input:**

```
Analyze 00700.HK Tencent Holdings.
```

**Output Includes:**
- Complete data pack (market data + income statement + balance sheet + cash flow)
- Industry competitive landscape and moat analysis
- Multi-method valuation (DCF / relative / asset restatement)
- Risk checklist
- Half-Kelly position sizing and buy price range

**Accounting Standards:** HKFRS / IFRS, CN-GAAP, US-GAAP

---

## Sample Reports

The [`reports/`](reports/) directory contains 40+ individual stock analysis reports across HK, A-share, and US markets for reference.

- **Cigar Butt Reports (20)**: China Dongxiang, Pou Sheng International, Financial Street Property, HK Ferry, China Education Group, etc.
- **Turtle Strategy Reports (20)**: Bull Group, NetEase, Gree Electric, Wuliangye, Feihe, Trip.com (TCOM), Duolingo (DUOL), etc.

---

## Directory Structure

```
Stock_Analyze_Prompts/
├── cigbutt/
│   ├── index.md                      — Framework overview
│   └── 烟蒂股prompt_v1.8.md          — Current prompt
├── turtle_framework/
│   ├── index.md                      — Framework overview
│   └── 乌龟战法_v0.15/
│       ├── coordinator.md            — Coordinator (main agent)
│       ├── phase1_数据采集.md         — Sub-agent 1: Data Collection
│       ├── phase2_PDF解析.md          — Sub-agent 2: PDF Parsing
│       └── phase3_分析与报告.md       — Sub-agent 3: Analysis & Report
└── reports/
    ├── cigbutt_reports/              — Cigar Butt analysis reports (20)
    └── turtle_reports/               — Turtle Strategy reports (20)
```
