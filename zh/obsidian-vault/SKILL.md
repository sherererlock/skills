---
name: obsidian-vault
description: 使用维基链接 (wikilinks) 和索引笔记在 Obsidian 仓库中搜索、创建和管理笔记。当用户希望在 Obsidian 中查找、创建或整理笔记时使用。
---

# Obsidian 仓库 (Vault)

## 仓库位置

`/mnt/d/Obsidian Vault/AI Research/`

根目录下基本保持扁平化结构。

## 命名规范

- **索引笔记 (Index notes)**：汇总相关主题（例如：`Ralph Wiggum Index.md`、`Skills Index.md`、`RAG Index.md`）
- 所有笔记名称均使用**首字母大写 (Title case)**
- 不使用文件夹进行整理，而是使用链接和索引笔记

## 链接

- 使用 Obsidian 的 `[[wikilinks]]` 语法：`[[Note Title]]`
- 笔记在底部链接到其依赖项或相关笔记
- 索引笔记仅由 `[[wikilinks]]` 列表组成

## 工作流

### 搜索笔记

```bash
# 按文件名搜索
find "/mnt/d/Obsidian Vault/AI Research/" -name "*.md" | grep -i "keyword"

# 按内容搜索
grep -rl "keyword" "/mnt/d/Obsidian Vault/AI Research/" --include="*.md"
```

或者直接在仓库路径上使用 Grep/Glob 工具。

### 创建新笔记

1. 文件名使用**首字母大写**
2. 将内容作为一个学习单元进行编写（遵循仓库规则）
3. 在底部添加指向相关笔记的 `[[wikilinks]]`
4. 如果是编号序列的一部分，请使用层级编号方案

### 查找相关笔记

在整个仓库中搜索 `[[Note Title]]` 以查找反向链接 (backlinks)：

```bash
grep -rl "\\[\\[Note Title\\]\\]" "/mnt/d/Obsidian Vault/AI Research/"
```

### 查找索引笔记

```bash
find "/mnt/d/Obsidian Vault/AI Research/" -name "*Index*"
```
