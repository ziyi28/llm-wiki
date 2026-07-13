---
name: ingest
description: 把 raw/ 中的新来源摄入 wiki：写来源摘要页、创建/更新相关概念与实体页、更新 index、追加 log、验证链接。当用户要求 ingest/摄入/处理/入库某个来源，或向 raw/ 添加新资料后要求处理时使用。
argument-hint: [raw/ 下的文件名（可留空，自动找未入库的来源）]
---

把新来源编译进 wiki。目标来源：$ARGUMENTS

## 0. 定位来源

- 给了文件名：目标即 `raw/<文件名>`（不带路径时自行补全，确认文件存在）。
- 留空：找出 `raw/` 中尚未入库的 markdown——对比 `raw/*.md` 与各 sources 页 frontmatter 的 `sources:` 字段。恰好一个未入库就处理它；多个则列出问用户；零个则报告「没有待摄入的来源」。

## 1. 通读与定位关联

1. 完整读取来源。含图片（`raw/assets/`）时先读全文，再按需逐张查看关键图片。
2. 读 `wiki/index.md`，判断来源与哪些已有页面相关、哪些新概念/实体值得建页。

## 2. 与用户对焦

用 5-8 个要点概括来源核心内容，说明打算新建哪些页、更新哪些页、有无与现有页面矛盾之处，等用户确认侧重后再动笔。
**例外**：用户明确要求「直接处理」或一次给了多个来源（批量模式）时跳过此步。

## 3. 写入（遵循 CLAUDE.md 页面格式）

1. `wiki/sources/` 新建来源摘要页：要点、主张、与已有内容的关系，wikilink 链向所有相关页与衍生页。
2. 创建/更新 concepts、entities 页：
   - 新概念/实体 → 建页（type 与目录对应，frontmatter 齐全）。
   - 已有页面 → 融合新信息，更新 `updated` 日期，`sources:` 追加本来源。
   - **新旧矛盾显式标注**：「来源 A 称 X；来源 B（较新）称 Y」，同时链接两个来源页。
   - 一个来源触及 10-15 页属正常，不要吝啬更新。
3. 更新 `wiki/index.md`：新页登记到对应分类（一行摘要），更新头部的页面计数与日期。
4. `wiki/log.md` **末尾**追加条目（日期用当天）：

   ```
   ## [YYYY-MM-DD] ingest | 来源标题
   - 来源、新建页、更新页各一行
   ```

## 4. 验证（必做；发现问题修复后重跑）

```bash
echo "=== 死链（应无输出）===" && comm -23 \
  <(grep -rhoE '\[\[[^]|#]+' wiki --include='*.md' | sed 's/^\[\[//' | LC_ALL=C sort -u) \
  <(find raw wiki -name '*.md' -exec basename {} .md \; | LC_ALL=C sort -u)
echo "=== index 覆盖（应无输出）===" && for f in wiki/sources/*.md wiki/concepts/*.md wiki/entities/*.md wiki/notes/*.md; do \
  [ -e "$f" ] || continue; b=$(basename "$f" .md); \
  grep -qF "[[$b" wiki/index.md || echo "index 缺少：$b"; done
echo "=== frontmatter（应无输出）===" && find wiki -name '*.md' | while read -r f; do \
  head -1 "$f" | grep -q '^---' || echo "缺 frontmatter: $f"; done
```

## 5. 汇报

总结：新建 X 页 / 更新 Y 页、发现的矛盾或亮点、建议的追问方向。不要主动 git commit（用户要求时才提交）。
