# 乌龟战法 (Turtle Framework)

乌龟战法 AI prompt，系统化多 Agent 投资分析框架，当前版本 v0.15。

适用市场：港股、A股、美股全市场。框架采用半凯利仓位模型，覆盖 HKFRS / IFRS、CN-GAAP、US-GAAP 三套会计准则。

## 框架架构（v0.15）

v0.15 采用多 Agent 协作架构，分四个模块：

| 模块 | 职责 |
|---|---|
| [Coordinator](乌龟战法_v0.15/coordinator.md) | 统筹各 Agent 工作流，任务分发与结果汇总 |
| [Phase 1 — 数据采集](乌龟战法_v0.15/phase1_数据采集.md) | yfinance MCP + WebSearch 采集市场与财务数据 |
| [Phase 2 — PDF 解析](乌龟战法_v0.15/phase2_PDF解析.md) | 解析年报、中报 PDF，提取结构化财务数据 |
| [Phase 3 — 分析与报告](乌龟战法_v0.15/phase3_分析与报告.md) | 生成完整投资分析报告，含估值与仓位建议 |
