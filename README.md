# Stock Analyze Prompts

<div align="center">

[简体中文](#简体中文) | [English](#english) | [Français](#français)

</div>

---

<a id="简体中文"></a>

## 简体中文

股票分析 AI prompt 集合，策略灵感来源于 B 站 UP 主「史诗级韭菜」，提供两套系统化的价值投资分析框架，适配 Claude 等大语言模型使用。

### 框架一：烟蒂股prompt（v1.8）

专为港股、A 股低估值股票设计的量化分析框架。核心逻辑基于格雷厄姆式净资产折价，系统性识别三类烟蒂股子类型，执行 22 项 Fact Check，输出 T 级安全边际评级与半凯利仓位建议。

**支持的三类子类型：**

| 子类型 | 核心逻辑 |
|---|---|
| A — 资产折价型 | 净现金/总资产折价，市值低于清算价值 |
| B — 子公司解锁型 | 上市子公司市值超过母公司总市值，隐蔽资产待解锁 |
| C — 回购/私有化型 | 持续回购或私有化预期，价值释放路径明确 |

**使用方法：**

1. 在 Claude（或其他 LLM）对话中，将 [`cigbutt/烟蒂股prompt_v1.8.md`](cigbutt/烟蒂股prompt_v1.8.md) 的全文内容作为 System Prompt 粘贴
2. 提供标的信息：股票代码或名称，加上至少 2 期财报（支持 PDF 上传或文字表格粘贴）
3. AI 自动通过 yfinance MCP / WebSearch 获取实时市值、股价、股息率等数据，生成完整 Markdown 分析报告

**输入示例：**

```
分析 3328.HK 交通银行，已上传 2023 年报和 2024 中报 PDF。
```

**输出包含：**
- 资产垫底（T 级安全边际）计算
- 三类子类型判定及核心条件逐项核查
- 22 项 Fact Check（含 Data 型 / Risk 型警告分类）
- 综合评级（A / B / C / 否决）
- 半凯利建仓仓位建议

**会计准则支持：** HKFRS / IFRS、CN-GAAP

---

### 框架二：乌龟战法（v0.15）

系统化多 Agent 投资分析框架，支持港股、A 股、美股全市场。采用 Coordinator + 三阶段 Sub-agent 协作架构，可对任意上市公司生成深度投资分析报告。

**分析流程：**

```
用户输入（股票代码 + 可选 PDF 年报）
        ↓
  Coordinator — 解析输入，自动调度各阶段
        ↓
  Phase 1 — 数据采集
  (yfinance MCP 获取市场数据、财务报表、股息历史)
        ↓
  Phase 2 — PDF 解析
  (提取年报/中报中的结构化财务数据)
        ↓
  Phase 3 — 分析与报告
  (估值分析、竞争优势、风险评估、半凯利仓位建议)
```

**使用方法（Claude Projects / API）：**

1. 配置 yfinance MCP（或其他股票数据 MCP 工具）
2. 将 [`coordinator.md`](turtle_framework/乌龟战法_v0.15/coordinator.md) 作为主 Agent 的 System Prompt
3. 将 Phase 1–3 分别配置为对应 Sub-agent 的 System Prompt
4. 在主对话中输入股票代码（可附上 PDF 年报）

**输入示例：**

```
分析 00700.HK 腾讯控股。
```

或上传年报 PDF 后：

```
分析 600519.SH 贵州茅台，已上传 2024 年报。
```

**输出包含：**
- 完整数据包（市场数据 + 财务三表）
- 行业竞争格局与护城河分析
- 多维度估值（DCF / 相对估值 / 资产重估）
- 风险清单
- 半凯利仓位建议与买入价格区间

**会计准则支持：** HKFRS / IFRS、CN-GAAP、US-GAAP

---

### 分析报告

[`reports/`](reports/) 目录包含已生成的 40+ 份个股分析报告，覆盖港股、A 股、美股，供参考对比。

- **烟蒂股分析报告（20 份）**：中国动向、宝胜国际、金融街物业、香港小轮、中教控股等
- **乌龟战法分析报告（20 份）**：公牛集团、网易、格力电器、五粮液、飞鹤乳业、腾讯（TCOM 携程）、多邻国（DUOL）等

---

### 目录结构

```
Stock_Analyze_Prompts/
├── cigbutt/
│   ├── index.md                      — 框架说明
│   └── 烟蒂股prompt_v1.8.md          — 当前版本 prompt
├── turtle_framework/
│   ├── index.md                      — 框架说明
│   └── 乌龟战法_v0.15/
│       ├── coordinator.md            — 协调器（主 Agent）
│       ├── phase1_数据采集.md         — Sub-agent 1
│       ├── phase2_PDF解析.md          — Sub-agent 2
│       └── phase3_分析与报告.md       — Sub-agent 3
└── reports/
    ├── cigbutt_reports/              — 烟蒂股分析报告（20 份）
    └── turtle_reports/               — 乌龟战法分析报告（20 份）
```

---

<a id="english"></a>

## English

A collection of AI prompts for stock analysis, providing two systematic value-investing frameworks designed for use with Claude and other LLMs. Strategy concepts inspired by the Bilibili creator "史诗级韭菜".

### Framework 1: Cigar Butt Prompt (v1.8)

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

### Framework 2: Turtle Strategy (v0.15)

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

### Sample Reports

The [`reports/`](reports/) directory contains 40+ individual stock analysis reports across HK, A-share, and US markets for reference.

- **Cigar Butt Reports (20)**: China Dongxiang, Pou Sheng International, Financial Street Property, HK Ferry, China Education Group, etc.
- **Turtle Strategy Reports (20)**: Bull Group, NetEase, Gree Electric, Wuliangye, Feihe, TCOM Trip.com, DUOL Duolingo, etc.

---

### Directory Structure

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

---

<a id="français"></a>

## Français

Une collection de prompts IA pour l'analyse boursière, proposant deux cadres d'investissement de valeur systématiques conçus pour Claude et d'autres LLM. Concepts stratégiques inspirés du créateur Bilibili « 史诗级韭菜 ».

### Cadre 1 : Prompt Mégots de Cigares (v1.8)

Un cadre d'analyse quantitative pour les actions sous-évaluées cotées à Hong Kong et sur les marchés A. Construit sur la logique de décote sur actif net à la Graham, il identifie systématiquement trois sous-types de « mégots de cigares », exécute 22 vérifications factuelles et fournit une note de marge de sécurité par paliers avec un dimensionnement de position à demi-Kelly.

**Trois Sous-types Couverts :**

| Sous-type | Logique Principale |
|---|---|
| A — Décote sur Actifs | Décote sur trésorerie nette / actifs ; capitalisation boursière inférieure à la valeur de liquidation |
| B — Déverrouillage de Filiale | La capitalisation de la filiale cotée dépasse la capitalisation totale de la société mère |
| C — Rachat / Privatisation | Rachats en cours ou anticipation de privatisation avec un chemin de libération de valeur clair |

**Mode d'Emploi :**

1. Collez le contenu intégral de [`cigbutt/烟蒂股prompt_v1.8.md`](cigbutt/烟蒂股prompt_v1.8.md) comme System Prompt dans Claude (ou un autre LLM)
2. Fournissez la cible : code boursier ou nom de la société, ainsi qu'au moins 2 périodes de rapports financiers (upload PDF ou tableau collé)
3. L'IA récupère en temps réel la capitalisation, le cours, le rendement du dividende, etc. via yfinance MCP / WebSearch et génère un rapport d'analyse complet en Markdown

**Exemple d'Entrée :**

```
Analyser 3328.HK Bank of Communications. J'ai joint les PDF du rapport annuel 2023 et du rapport semestriel 2024.
```

**Livrables :**
- Calcul du coussin d'actifs (marge de sécurité de niveau T)
- Détermination du sous-type avec vérification de chaque condition
- 22 vérifications factuelles (classification des alertes : type Données vs type Risque)
- Note globale (A / B / C / Rejeté)
- Recommandation de dimensionnement de position à demi-Kelly

**Normes Comptables Supportées :** HKFRS / IFRS, CN-GAAP

---

### Cadre 2 : Stratégie Tortue (v0.15)

Un cadre d'analyse d'investissement multi-agents systématique couvrant les marchés de Hong Kong, des actions A et américains. Utilise une architecture Coordinateur + trois phases de sous-agents pour générer des rapports d'analyse approfondis pour toute société cotée.

**Pipeline d'Analyse :**

```
Entrée Utilisateur (code boursier + PDF rapport annuel optionnel)
        ↓
  Coordinateur — analyse l'entrée, planifie toutes les phases
        ↓
  Phase 1 — Collecte de Données
  (yfinance MCP : données de marché, états financiers, historique des dividendes)
        ↓
  Phase 2 — Analyse PDF
  (extraction structurée des rapports annuels / semestriels)
        ↓
  Phase 3 — Analyse et Rapport
  (valorisation, avantage concurrentiel, évaluation des risques, dimensionnement demi-Kelly)
```

**Mode d'Emploi (Claude Projects / API) :**

1. Configurer yfinance MCP (ou un outil MCP équivalent pour données boursières)
2. Définir [`coordinator.md`](turtle_framework/乌龟战法_v0.15/coordinator.md) comme System Prompt de l'agent principal
3. Configurer les Phases 1–3 comme System Prompts des sous-agents respectifs
4. Saisir un code boursier dans la conversation principale (joindre optionnellement un PDF de rapport annuel)

**Exemple d'Entrée :**

```
Analyser 00700.HK Tencent Holdings.
```

**Livrables :**
- Pack de données complet (données de marché + compte de résultat + bilan + flux de trésorerie)
- Analyse du paysage concurrentiel et des avantages concurrentiels
- Valorisation multi-méthodes (DCF / relative / réévaluation d'actifs)
- Liste des risques
- Dimensionnement de position à demi-Kelly et fourchette de prix d'achat

**Normes Comptables Supportées :** HKFRS / IFRS, CN-GAAP, US-GAAP

---

### Rapports d'Exemple

Le répertoire [`reports/`](reports/) contient plus de 40 rapports d'analyse boursière individuels couvrant les marchés de Hong Kong, des actions A et américains, disponibles à titre de référence.

- **Rapports Mégots de Cigares (20)** : China Dongxiang, Pou Sheng International, Financial Street Property, HK Ferry, China Education Group, etc.
- **Rapports Stratégie Tortue (20)** : Bull Group, NetEase, Gree Electric, Wuliangye, Feihe, TCOM Trip.com, DUOL Duolingo, etc.

---

### Structure du Répertoire

```
Stock_Analyze_Prompts/
├── cigbutt/
│   ├── index.md                      — Présentation du cadre
│   └── 烟蒂股prompt_v1.8.md          — Prompt version actuelle
├── turtle_framework/
│   ├── index.md                      — Présentation du cadre
│   └── 乌龟战法_v0.15/
│       ├── coordinator.md            — Coordinateur (agent principal)
│       ├── phase1_数据采集.md         — Sous-agent 1 : Collecte de données
│       ├── phase2_PDF解析.md          — Sous-agent 2 : Analyse PDF
│       └── phase3_分析与报告.md       — Sous-agent 3 : Analyse et rapport
└── reports/
    ├── cigbutt_reports/              — Rapports mégots de cigares (20)
    └── turtle_reports/               — Rapports Stratégie Tortue (20)
```
