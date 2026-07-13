# LLM Wiki — Schema

本仓库是一个「LLM 维护的知识库」，实例化自 [LLM Wiki 模式](raw/llm-wiki.md)。分工：

- **用户**：提供来源（放入 `raw/`）、提问、把关方向。
- **LLM**：全部的写作、摘要、交叉引用、归档、簿记——即 `wiki/` 下所有内容的创建与维护。

用户在 Obsidian 中把本目录作为 vault 打开，浏览页面和关系图谱；LLM 在 Claude Code 中读写文件。

## 目录结构

| 路径 | 作用 | 写权限 |
|------|------|--------|
| `raw/` | 原始资料：文章、论文、剪藏、笔记。**只读——LLM 永不修改** | 用户 |
| `raw/assets/` | Obsidian 剪藏文章的本地图片附件 | Obsidian |
| `wiki/首页.md` | 总览页：知识库说明与关键入口 | LLM |
| `wiki/index.md` | 内容目录：每页一行摘要，按类别组织，**每次 ingest 后更新** | LLM |
| `wiki/log.md` | 时间线日志，**append-only**，新条目追加到文件末尾 | LLM |
| `wiki/sources/` | 来源摘要页：每个 raw 来源对应一页 | LLM |
| `wiki/concepts/` | 概念页：方法、理论、模式、术语 | LLM |
| `wiki/entities/` | 实体页：人物、工具、组织、作品 | LLM |
| `wiki/notes/` | 查询产出：分析、比较、综合（有价值的答案回填于此） | LLM |
| `CLAUDE.md` | 本 schema，随使用与用户共同演化 | 双方 |

## 页面格式

每个 wiki 页面以 YAML frontmatter 开头：

```yaml
---
type: source | concept | entity | note | meta
tags: [标签]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: [raw/来源文件名.md]
---
```

- `type` 与所在目录对应；`meta` 仅用于 首页 / index / log 三个特殊页。
- `sources` 列出该页内容依据的原始资料，便于溯源和 Dataview 查询。
- 页面正文用**中文**，技术术语（RAG、ingest、frontmatter …）保留英文。
- 交叉引用一律用 wikilink：`[[页面名]]` 或 `[[页面名|显示文字]]`——Obsidian 图谱的边由此而来。正文提到已有页面对应的概念/实体时应加链接。
- 文件名：中文通用名；专有名词保留英文（如 `RAG.md`、`Obsidian.md`）。

## 工作流

### Ingest（摄入新来源）

用户把新资料放进 `raw/` 并要求处理时：

1. 通读来源全文（含图片时先读文本，再按需逐张查看图片）。
2. 与用户简要讨论核心要点，确认侧重点。
3. 在 `wiki/sources/` 写来源摘要页（要点、主张、与已有内容的关系）。
4. 创建或更新相关的 concepts / entities 页：新概念建页；已有页面融合新信息，**新旧矛盾之处显式标注**（同时引用两个来源）。一个来源触及 10-15 个页面是正常的。
5. 更新 `wiki/index.md`（含页面计数与日期）。
6. 在 `wiki/log.md` 末尾追加条目。

### Query（查询）

1. 先读 `wiki/index.md` 定位相关页面，再读页面本身；必要时回查 raw 来源。
2. 综合作答，注明依据的页面/来源。
3. 有沉淀价值的答案（比较、分析、新联系）写入 `wiki/notes/`，更新 index、追加 log——让探索成果与来源一样复利。

### Lint（健康检查）

用户要求体检时，检查并报告：

- 页面之间的矛盾；被新来源推翻的过时声明
- 孤儿页（无入链）；被多处提到却没有自己页面的概念
- 缺失的交叉引用；index 与实际文件不一致；死链（wikilink 指向不存在的页面）
- 值得追问的问题、值得补充的来源

### Log 条目格式

```
## [YYYY-MM-DD] ingest|query|lint | 标题
- 一句话说明 + 涉及页面
```

固定前缀使日志可 grep：`grep "^## \[" wiki/log.md`。

## 约束

- `raw/` 只读；发现来源本身有问题时在 wiki 页面标注，不改原文。
- 删除或重命名 wiki 页面前，先全库搜索指向它的 wikilink 一并处理。
- 本文件由双方共同演化：发现更好的约定，与用户确认后更新此处。
