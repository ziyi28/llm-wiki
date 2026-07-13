---
type: entity
tags: [工具, 搜索]
created: 2026-07-12
updated: 2026-07-12
sources: [raw/llm-wiki.md]
---

# qmd

[tobi/qmd](https://github.com/tobi/qmd)：面向 markdown 文件的本地搜索引擎——BM25 + 向量的混合检索，外加 LLM 重排序，全部在本机运行。

## 在本模式中的位置

小规模下 index 文件足够导航（见 [[索引与日志]]）；当 wiki 大到 index 不堪重负时，qmd 是现成的升级选项：

- **CLI**：LLM 直接 shell 调用；
- **MCP server**：LLM 作为原生工具使用。

也可以让 LLM 现写一个更简单的搜索脚本，按需生长——[[LLM-Wiki模式]] 的一贯思路是一切组件按需引入。

*来源：[[LLM-Wiki模式文档]]*
