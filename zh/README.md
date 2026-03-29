# 智能体技能

一系列旨在扩展规划、开发和工具链能力的智能体技能集合。

## 规划与设计

这些技能可帮助您在编写代码前深入思考问题。

- **write-a-prd** — 通过互动式访谈、代码库探索和模块设计来创建 PRD（产品需求文档），并作为 GitHub Issue 提交。

  ```
  npx skills@latest add mattpocock/skills/write-a-prd
  ```

- **prd-to-plan** — 使用“曳光弹”式的垂直切片方法，将 PRD 转化为多阶段实施计划。

  ```
  npx skills@latest add mattpocock/skills/prd-to-plan
  ```

- **prd-to-issues** — 使用垂直切片方法，将 PRD 拆解为可独立领取的 GitHub Issue。

  ```
  npx skills@latest add mattpocock/skills/prd-to-issues
  ```

- **grill-me** — 对计划或设计进行打破砂锅问到底式的拷问，直到决策树的每个分支都得到妥善处理。

  ```
  npx skills@latest add mattpocock/skills/grill-me
  ```

- **design-an-interface** — 使用并行的子智能体，为一个模块生成多种截然不同的接口设计。

  ```
  npx skills@latest add mattpocock/skills/design-an-interface
  ```

- **request-refactor-plan** — 通过用户访谈创建包含微小提交（tiny commits）的详细重构计划，然后将其作为 GitHub Issue 提交。

  ```
  npx skills@latest add mattpocock/skills/request-refactor-plan
  ```

## 开发

这些技能可帮助您编写、重构和修复代码。

- **tdd** — 采用“红-绿-重构”循环的测试驱动开发。一次构建一个垂直切片的功能或修复漏洞。

  ```
  npx skills@latest add mattpocock/skills/tdd
  ```

- **triage-issue** — 通过探索代码库调查漏洞，找出根本原因，并提交一个包含基于 TDD 修复计划的 GitHub Issue。

  ```
  npx skills@latest add mattpocock/skills/triage-issue
  ```

- **improve-codebase-architecture** — 探索代码库寻找架构改进的机会，重点是深化浅层模块和提高可测试性。

  ```
  npx skills@latest add mattpocock/skills/improve-codebase-architecture
  ```

- **migrate-to-shoehorn** — 将测试文件从 `as` 类型断言迁移至 @total-typescript/shoehorn。

  ```
  npx skills@latest add mattpocock/skills/migrate-to-shoehorn
  ```

- **scaffold-exercises** — 创建包含章节、问题、解决方案和解释说明的练习题目录结构。

  ```
  npx skills@latest add mattpocock/skills/scaffold-exercises
  ```

## 工具链与环境配置

- **setup-pre-commit** — 配置 Husky 的 pre-commit 钩子，包含 lint-staged、Prettier、类型检查和测试功能。

  ```
  npx skills@latest add mattpocock/skills/setup-pre-commit
  ```

- **git-guardrails-claude-code** — 配置 Claude Code 钩子，在执行前拦截危险的 Git 命令（如 push、reset --hard、clean 等）。

  ```
  npx skills@latest add mattpocock/skills/git-guardrails-claude-code
  ```

## 写作与知识管理

- **write-a-skill** — 创建具有良好结构、渐进式呈现及捆绑资源的新技能。

  ```
  npx skills@latest add mattpocock/skills/write-a-skill
  ```

- **edit-article** — 通过重构章节、提高清晰度和精简文字来编辑及润色文章。

  ```
  npx skills@latest add mattpocock/skills/edit-article
  ```

- **ubiquitous-language** — 从当前对话中提取领域驱动设计 (DDD) 风格的通用语言词汇表。

  ```
  npx skills@latest add mattpocock/skills/ubiquitous-language
  ```

- **obsidian-vault** — 在 Obsidian 知识库中搜索、创建和管理带有维基链接（wikilinks）和索引笔记的笔记。

  ```
  npx skills@latest add mattpocock/skills/obsidian-vault
  ```
