---
name: scaffold-exercises
description: 创建包含章节、问题、解答和讲解且通过代码检查（linting）的练习目录结构。当用户想要搭建练习脚手架、创建练习存根（stubs）或设置新的课程章节时使用。
---

# 搭建练习脚手架 (Scaffold Exercises)

创建能够通过 `pnpm ai-hero-cli internal lint` 检查的练习目录结构，然后使用 `git commit` 进行提交。

## 目录命名

- **章节 (Sections)**：位于 `exercises/` 目录下的 `XX-section-name/` 格式（例如：`01-retrieval-skill-building`）
- **练习 (Exercises)**：位于章节目录下的 `XX.YY-exercise-name/` 格式（例如：`01.03-retrieval-with-bm25`）
- 章节编号 = `XX`，练习编号 = `XX.YY`
- 命名采用短横线分隔命名法（小写字母，使用连字符）

## 练习变体

每个练习至少需要包含以下子文件夹之一：

- `problem/` - 带有 TODO 待办事项的学生工作区
- `solution/` - 参考实现
- `explainer/` - 概念讲解材料，无 TODO 待办事项

在创建存根（stubbing）时，除非计划中另有指定，否则默认使用 `explainer/`。

## 必需文件

每个子文件夹（`problem/`、`solution/`、`explainer/`）都需要一个 `readme.md`，且满足以下条件：

- **非空**（必须有实际内容，哪怕只有一行标题也可以）
- 没有失效的链接

创建存根时，创建一个只包含标题和描述的极简 readme 文件：

```md
# 练习标题

此处填写描述
```

如果子文件夹包含代码，它还需要一个 `main.ts`（>1 行）。但对于存根而言，仅包含 readme 的练习也是可以的。

## 工作流程

1. **解析计划** - 提取章节名称、练习名称以及变体类型
2. **创建目录** - 为每个路径执行 `mkdir -p`
3. **创建 readme 存根** - 每个变体文件夹下创建一个带有标题的 `readme.md`
4. **运行代码检查 (Lint)** - 运行 `pnpm ai-hero-cli internal lint` 进行验证
5. **修复所有错误** - 反复迭代直到通过代码检查

## 代码检查 (Lint) 规则总结

代码检查工具 (`pnpm ai-hero-cli internal lint`) 会检查：

- 每个练习都有子文件夹（`problem/`、`solution/`、`explainer/`）
- 至少存在 `problem/`、`explainer/` 或 `explainer.1/` 中的一个
- 主子文件夹中存在 `readme.md` 且非空
- 没有 `.gitkeep` 文件
- 没有 `speaker-notes.md` 文件
- readme 文件中没有失效链接
- readme 文件中没有 `pnpm run exercise` 命令
- 除非是仅有 readme 的练习，否则每个子文件夹都必须有 `main.ts`

## 移动/重命名练习

在重新编号或移动练习时：

1. 使用 `git mv`（而不是 `mv`）来重命名目录 - 这样可以保留 git 历史记录
2. 更新数字前缀以保持顺序
3. 移动后重新运行代码检查

示例：

```bash
git mv exercises/01-retrieval/01.03-embeddings exercises/01-retrieval/01.04-embeddings
```

## 示例：根据计划创建存根

假设有如下计划：

```
Section 05: Memory Skill Building
- 05.01 Introduction to Memory
- 05.02 Short-term Memory (explainer + problem + solution)
- 05.03 Long-term Memory
```

创建目录：

```bash
mkdir -p exercises/05-memory-skill-building/05.01-introduction-to-memory/explainer
mkdir -p exercises/05-memory-skill-building/05.02-short-term-memory/{explainer,problem,solution}
mkdir -p exercises/05-memory-skill-building/05.03-long-term-memory/explainer
```

然后创建 readme 存根：

```
exercises/05-memory-skill-building/05.01-introduction-to-memory/explainer/readme.md -> "# Introduction to Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/explainer/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/problem/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/solution/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.03-long-term-memory/explainer/readme.md -> "# Long-term Memory"
```