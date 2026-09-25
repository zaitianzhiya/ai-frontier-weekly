# 信息渠道调研 — AI 能力前沿课题

> 课题：当前最强的 AI 可以强到什么程度、能实现哪些不可置信的任务
> 调研日期：2026-09-25

## 渠道权重方法论

本项目的可信度模型是**跨生态交叉验证**：同一事件被越多独立生态报道，置信度越高。权重设计遵循三个原则：

1. **独立性优先**：独立第三方验证（评估机构、学术）权重 ≥ 官方发布（存在宣传倾向）
2. **一手优先**：官方博客/论文 > 榜单聚合 > 媒体转述 > 社区传闻
3. **可自动化优先**：所有渠道必须能通过 DDG 新闻搜索 / arXiv API / GitHub API 无密钥或低成本采集

## 渠道清单与权重

### Tier 1 — 一手/权威源（每个 40 分）

| 生态 | 权重 | 渠道 | 采集方式 | 入选理由 |
|---|---|---|---|---|
| 实验室官方 | 1.0 | OpenAI / Anthropic / Google DeepMind / Meta AI / xAI / 开源前沿(Qwen/DeepSeek) | DDG 新闻 | 能力发布的一手来源；2026 年 GPT-5.4、Gemini 3.1、Claude 5 系列的能力边界都由官方率先公布 |
| 能力评估机构 | 1.0 | Epoch AI / METR / ARC Prize Foundation / Scale AI SEAL / Artificial Analysis / Stanford HAI | DDG 新闻 | 第三方独立验证，交叉验证的黄金标准；Epoch 的能力指数、METR 的自主时域、SEAL 的 HLE 榜单是"AI 有多强"最硬的数据 |
| 学术界 | 1.0 | arXiv (cs.AI/cs.CL/cs.LG) / Nature·Science AI 板块 | arXiv API / DDG | 同行评议论文（如 HLE 论文发表于 Nature Vol 649）；arXiv 是能力研究的第一落点 |

### Tier 2 — 聚合/媒体源（每个 25 分）

| 生态 | 权重 | 渠道 | 采集方式 | 入选理由 |
|---|---|---|---|---|
| 基准聚合 | 0.9 | LMSYS Chatbot Arena / benchlm.ai / Vals AI / llm-stats.com | DDG 新闻 | 榜单聚合提供跨模型横向对比；benchlm 每日同步 SWE-bench Verified/Terminal-Bench/OSWorld/ARC-AGI-2/HLE |
| 国际媒体 | 0.8 | MIT Technology Review / The Verge / TechCrunch AI | DDG 新闻 | 深度分析与能力叙事；MIT TR 的评估方法论报道质量最高 |
| 中文媒体 | 0.8 | 机器之心 / 量子位 | DDG 新闻 | 中文圈一手深度报道，覆盖国产模型与开源生态 |
| 开发者生态 | 0.8 | GitHub Trending (ai-agent/llm/ai 话题) | GitHub API | 能力落地的先行指标：热门 AI 项目反映"什么任务刚被解锁" |
| 社区聚合 | 0.7 | Hacker News / Reddit (r/MachineLearning, r/LocalLLaMA) / Hugging Face | DDG 新闻 | 噪音高但有首发信息价值（泄露、复现、实测）；权重刻意压低 |

## 权重组合的实际效果

| 事件类型 | 交叉验证来源 | 得分示例 | 等级 |
|---|---|---|---|
| 官方发布 + 独立复测 + 学术论文 | OpenAI + Vals AI + arXiv | 40×1.0 + 40×1.0 + 40×1.0 = 120→100 | A |
| 官方发布 + 媒体确认 | OpenAI + The Verge | 40×1.0 + 25×0.8 = 60 | B |
| 仅评估机构报告 | METR | 40×1.0 = 40 | C |
| 仅社区传闻 | Reddit | 25×0.7 = 17.5 | D |

## 调研依据（2026-09 检索）

- Epoch AI 能力指数：前沿能力提升速度约 15.5 分/年（2024 年前为 8 分/年）
- METR 自主时域：2019 年 2 秒 → 2026 年 4 月约 17 小时，129 天翻倍
- 基准饱和曲线：FrontierMath <2%→25%、ARC-AGI ~5%→88%、SWE-bench 2%→70%、HLE 3%→27%
- 独立复测价值：GPT-5.5 的 SWE-bench 自报 88.7% vs 独立实测 82.6% —— 官方数据需要评估机构交叉验证
- ARC-AGI-3 使全部前沿模型得分 <1% —— "能力边界"正在被更快的新基准重新定义

## 维护说明

- 渠道增删改 `config/sources.yml`；权重调整 `config/quality.yml` 的 `ecosystem_weights`
- 当某个基准饱和（如 SWE-bench Verified 集中在 95-96%）时，应把关键词切换到替代基准（SWE-Bench Pro / ARC-AGI-3）
