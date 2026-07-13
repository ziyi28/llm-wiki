---
type: entity
tags: [工具]
created: 2026-07-12
updated: 2026-07-12
sources: [raw/llm-wiki.md]
---

# Obsidian

本地优先的 markdown 笔记应用，在 [[LLM-Wiki模式]] 中扮演**浏览界面**：LLM 在一侧编辑文件，用户在 Obsidian 里实时看结果——「Obsidian 是 IDE，LLM 是程序员，wiki 是代码库」。

## 在本模式中的用法

- **图谱视图（Graph view）**：看 wiki 形状的最佳方式——什么连着什么、哪些页面是枢纽、哪些是孤儿（配合 [[Lint]]）。本库已预置按目录着色的图谱配置。
- **Web Clipper 浏览器扩展**：把网页文章转成 markdown，快速进入 `raw/`，是 [[Ingest]] 的上游入口。
- **本地下载附件**：设置 → Files and links → 附件目录设为 `raw/assets`（本库已预置）；再到 Settings → Hotkeys 给「Download attachments for current file」绑一个快捷键（如 Ctrl+Shift+D）。剪藏后按一下，全部图片落到本地磁盘，LLM 即可直接查看，不怕外链失效。注意：LLM 无法一次读完图文混排的 markdown——先读文本，再按需逐张看图。
- **Dataview 插件**：对页面 frontmatter 跑查询、生成动态表格——本库所有页面都带 frontmatter，正是为此留的接口。
- **Marp 插件**：markdown 写幻灯片，[[Query]] 的答案可以直接输出成演示文稿。

*来源：[[LLM-Wiki模式文档]]*
