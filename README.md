# Stock Analyze Prompts

<div align="center">

[简体中文](README.md) | [English](README_en.md) | [Français](README_fr.md)

</div>

---

股票分析 AI prompt 集合，策略灵感来源于 B 站 UP 主「史诗级韭菜」，提供两套系统化的价值投资分析框架，适配 Claude 等大语言模型使用。

## 框架一：烟蒂股prompt（v1.8）

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

## 框架二：乌龟战法（v0.15）

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

**输出包含：**
- 完整数据包（市场数据 + 财务三表）
- 行业竞争格局与护城河分析
- 多维度估值（DCF / 相对估值 / 资产重估）
- 风险清单
- 半凯利仓位建议与买入价格区间

**会计准则支持：** HKFRS / IFRS、CN-GAAP、US-GAAP

---

## 分析报告

[`reports/`](reports/) 目录包含已生成的 40+ 份个股分析报告，覆盖港股、A 股、美股，供参考对比。

- **烟蒂股分析报告（20 份）**：中国动向、宝胜国际、金融街物业、香港小轮、中教控股等
- **乌龟战法分析报告（20 份）**：公牛集团、网易、格力电器、五粮液、飞鹤乳业、携程（TCOM）、多邻国（DUOL）等

---

## 目录结构

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
