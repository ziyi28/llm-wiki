---
name: lint
description: 给 wiki 做健康检查并修复：死链、孤儿页、index 不一致、frontmatter 缺失（脚本检查），以及页面矛盾、过时声明、缺失页面与交叉引用（语义检查）。当用户要求 lint/体检/检查/清理 wiki 时使用。
---

## 1. 机械检查（先跑脚本）

```bash
echo "=== A. 死链（wikilink 指向不存在的页面）===" && comm -23 \
  <(grep -rhoE '\[\[[^]|#]+' wiki --include='*.md' | sed 's/^\[\[//' | LC_ALL=C sort -u) \
  <(find raw wiki -name '*.md' -exec basename {} .md \; | LC_ALL=C sort -u)
echo "=== B. 孤儿页（除 index/log 外无入链）===" && for f in wiki/sources/*.md wiki/concepts/*.md wiki/entities/*.md wiki/notes/*.md; do \
  [ -e "$f" ] || continue; b=$(basename "$f" .md); \
  grep -rlF --include='*.md' "[[$b" wiki | grep -v -e 'wiki/index\.md' -e 'wiki/log\.md' -e "^$f\$" | grep -q . || echo "孤儿页：$f"; done
echo "=== C. index 覆盖（内容页应全部登记）===" && for f in wiki/sources/*.md wiki/concepts/*.md wiki/entities/*.md wiki/notes/*.md; do \
  [ -e "$f" ] || continue; b=$(basename "$f" .md); \
  grep -qF "[[$b" wiki/index.md || echo "index 缺少：$b"; done
echo "=== D. frontmatter ===" && find wiki -name '*.md' | while read -r f; do \
  head -1 "$f" | grep -q '^---' || echo "缺 frontmatter: $f"; done
echo "=== E. 最近日志 ===" && grep "^## \[" wiki/log.md | tail -5
```

注：孤儿页检测按页面名前缀匹配，存在同前缀页面（如 `LLM-Wiki模式` 与 `LLM-Wiki模式文档`）时可能漏报，语义检查阶段留意。

## 2. 语义检查（阅读页面）

读 `wiki/index.md` 与全部内容页（库变大后优先最近更新的页 + 枢纽页），寻找：

1. 页面之间的**矛盾**；被新来源**推翻的过时声明**。
2. 被多个页面提到却**还没有自己页面**的概念/实体 → 建页建议。
3. 正文提到已有页面却**没加 wikilink** 的地方。
4. **数据缺口**：值得 web 搜索或补充新来源的问题。

## 3. 报告与修复

按严重度输出报告：

- 🔴 **直接修复**：死链、index 不一致、frontmatter 缺失、缺失的 wikilink——修完重跑脚本确认归零。
- 🟡 **建议（待用户确认）**：孤儿页去留、矛盾如何裁决、建哪些新页。
- 💡 **探索**：建议追问的问题、值得寻找的新来源。

最后在 `wiki/log.md` 末尾追加：`## [YYYY-MM-DD] lint | 健康检查`（列出修复项与遗留建议）。不要主动 git commit（用户要求时才提交）。
