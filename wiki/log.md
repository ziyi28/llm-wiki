---
type: meta
tags: [日志]
created: 2026-07-12
updated: 2026-07-12
sources: []
---

# 日志

append-only 时间线，新条目追加到文件末尾。条目格式：`## [YYYY-MM-DD] ingest|query|lint | 标题`（可用 `grep "^## \[" wiki/log.md` 解析）。

## [2026-07-12] ingest | LLM Wiki 模式文档（知识库初始化）

- 来源：`raw/llm-wiki.md` —— 本知识库所遵循模式的说明文档，作为首个来源完成示范性 ingest。
- 新建来源页：[[LLM-Wiki模式文档]]
- 新建概念页：[[LLM-Wiki模式]]、[[RAG]]、[[三层架构]]、[[Ingest]]、[[Query]]、[[Lint]]、[[索引与日志]]
- 新建实体页：[[Obsidian]]、[[Memex]]、[[qmd]]
- 同时完成：目录结构、schema（CLAUDE.md）、[[首页]]、[[index|内容目录]]、Obsidian 图谱配置、git 仓库初始化。
