# CLAUDE.md — AI Frontier Weekly

## 项目目标

每周自动生成「AI 能力前沿周报」：追踪最强 AI 的能力边界（基准成绩、自主任务、科学发现等），回答"AI 现在能做什么不可置信的事"。

## 架构

```
run.py                      # 入口 (--mode weekly)
src/main.py                 # 编排: collect → filter → dedup → score → AI → render
src/collectors/real_search.py  # DDG 新闻(T1/T2) / arXiv(rss) / GitHub(api)
src/filters/{quality,dedup,scorer}.py
src/render/markdown_weekly.py  # 周报渲染
src/ai/{llm_client,deep_analyzer}.py
config/{sources,keywords,quality}.yml  # 全部可调参数
data/dedup_state.json       # 跨周去重状态（必须提交，CI 同步）
output/weekly/YYYY/YYYY-Www.md
```

## 关键约定

- **评分**：跨生态交叉验证。T1=40 分 / T2=25 分 × 生态权重，A≥80 / B≥60 / C≥30 / D 兜底
- **event_id**：归一化 URL 的 md5（不含来源），保证跨源合并
- **去重**：状态文件 `data/dedup_state.json` 随报告一起提交；渲染成功后才 save()
- **占位骨架**：任何采集失败产生的骨架记录带 `fallback` 标记，质量过滤阶段剔除
- **周号**：统一 ISO 周 `%G-W%V`（严禁 %Y-W%V）
- **REPORT_WEEK 环境变量**：补刊指定周（须匹配 `\d{4}-W\d{2}`，使用独立去重状态文件）
- **无 LLM key**：自动降级为数据版周报，不产出机器翻译垃圾

## 修改渠道/权重

- 渠道：`config/sources.yml`（tier/ecosystem/type/keywords）
- 权重：`config/quality.yml` 的 `ecosystem_weights`
- 分类：`config/quality.yml` 的 `categories` + `category_mapping`

## 测试

改完代码跑 `pytest tests/ -q`（22 个用例覆盖评分/过滤/去重/合并/分类/渲染）。
