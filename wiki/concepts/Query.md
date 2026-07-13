---
type: concept
tags: [工作流]
created: 2026-07-12
updated: 2026-07-12
sources: [raw/llm-wiki.md]
---

# Query

对 wiki 提问。LLM 先读 index 定位相关页面（见 [[索引与日志]]），读取后综合作答并附引用。

## 答案的形式

视问题而定：markdown 页面、比较表格、Marp 幻灯片、matplotlib 图表、画布……（Marp 见 [[Obsidian]] 的插件生态）

## 关键洞见：答案回填

**好答案不该消失在聊天记录里。**要过的比较、做过的分析、发现的联系——都值得作为新页面存回 wiki（本库放 `wiki/notes/`）。这样**探索与来源一样在知识库中复利**，是 [[LLM-Wiki模式]] 区别于普通问答的第二重积累。

## 相关

[[Ingest]] · [[Lint]]

*来源：[[LLM-Wiki模式文档]]*
