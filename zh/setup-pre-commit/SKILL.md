---
name: setup-pre-commit
description: 在当前仓库中设置带有 lint-staged (Prettier)、类型检查和测试的 Husky pre-commit 钩子。当用户想要添加 pre-commit 钩子、设置 Husky、配置 lint-staged 或添加提交时的格式化/类型检查/测试时使用。
---

# 设置 Pre-Commit 钩子

## 这将设置什么

- **Husky** pre-commit 钩子
- **lint-staged** 在所有暂存文件上运行 Prettier
- **Prettier** 配置（如果缺失）
- pre-commit 钩子中的 **typecheck** 和 **test** 脚本

## 步骤

### 1. 检测包管理器

检查是否存在 `package-lock.json` (npm)、`pnpm-lock.yaml` (pnpm)、`yarn.lock` (yarn)、`bun.lockb` (bun)。使用其中存在的任何一个。如果不明确，则默认使用 npm。

### 2. 安装依赖

作为 devDependencies 安装：

```
husky lint-staged prettier
```

### 3. 初始化 Husky

```bash
npx husky init
```

这会创建 `.husky/` 目录并将 `prepare: "husky"` 添加到 package.json 中。

### 4. 创建 `.husky/pre-commit`

写入此文件（Husky v9+ 不需要 shebang）：

```
npx lint-staged
npm run typecheck
npm run test
```

**适配**：将 `npm` 替换为检测到的包管理器。如果仓库的 package.json 中没有 `typecheck` 或 `test` 脚本，请省略这些行并告知用户。

### 5. 创建 `.lintstagedrc`

```json
{
  "*": "prettier --ignore-unknown --write"
}
```

### 6. 创建 `.prettierrc`（如果缺失）

仅在没有 Prettier 配置存在时创建。使用以下默认值：

```json
{
  "useTabs": false,
  "tabWidth": 2,
  "printWidth": 80,
  "singleQuote": false,
  "trailingComma": "es5",
  "semi": true,
  "arrowParens": "always"
}
```

### 7. 验证

- [ ] `.husky/pre-commit` 存在且可执行
- [ ] `.lintstagedrc` 存在
- [ ] package.json 中的 `prepare` 脚本为 `"husky"`
- [ ] `prettier` 配置存在
- [ ] 运行 `npx lint-staged` 以验证其是否正常工作

### 8. 提交

暂存所有已更改/创建的文件并使用以下信息提交：`Add pre-commit hooks (husky + lint-staged + prettier)`

这将运行新的 pre-commit 钩子 —— 这是一个验证一切正常工作的绝佳冒烟测试。

## 注意事项

- Husky v9+ 不需要钩子文件中的 shebang
- `prettier --ignore-unknown` 会跳过 Prettier 无法解析的文件（图片等）
- pre-commit 首先运行 lint-staged（快速，仅限于暂存文件），然后进行完整的类型检查和测试
