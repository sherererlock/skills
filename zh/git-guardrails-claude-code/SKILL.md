---
name: git-guardrails-claude-code
description: 设置 Claude Code 钩子以在执行前拦截危险的 git 命令（如 push、reset --hard、clean、branch -D 等）。当用户希望防止破坏性的 git 操作、添加 git 安全护栏或在 Claude Code 中拦截 git push/reset 时使用。
---

# 设置 Git 安全护栏

设置一个 PreToolUse 钩子，在 Claude 执行危险的 git 命令之前对其进行拦截和阻止。

## 拦截内容

- `git push`（包含 `--force` 在内的所有变体）
- `git reset --hard`
- `git clean -f` / `git clean -fd`
- `git branch -D`
- `git checkout .` / `git restore .`

当命令被拦截时，Claude 会看到一条消息，提示其无权访问并执行这些命令。

## 步骤

### 1. 询问作用域

询问用户：是**仅为当前项目**（`.claude/settings.json`）安装，还是为**所有项目**（`~/.claude/settings.json`）全局安装？

### 2. 复制钩子脚本

附带的脚本位于：[scripts/block-dangerous-git.sh](scripts/block-dangerous-git.sh)

根据作用域将其复制到目标位置：

- **项目级别**：`.claude/hooks/block-dangerous-git.sh`
- **全局级别**：`~/.claude/hooks/block-dangerous-git.sh`

使用 `chmod +x` 赋予其可执行权限。

### 3. 将钩子添加到设置文件

将其添加到相应的设置文件中：

**项目级别** (`.claude/settings.json`)：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/block-dangerous-git.sh"
          }
        ]
      }
    ]
  }
}
```

**全局级别** (`~/.claude/settings.json`)：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/hooks/block-dangerous-git.sh"
          }
        ]
      }
    ]
  }
}
```

如果设置文件已存在，请将该钩子合并到现有的 `hooks.PreToolUse` 数组中 —— 请勿覆盖其他设置。

### 4. 询问自定义需求

询问用户是否需要向拦截列表中添加或移除任何匹配模式，并据此修改复制的脚本。

### 5. 验证测试

运行快速测试：

```bash
echo '{"tool_input":{"command":"git push origin main"}}' | <path-to-script>
```

程序应以退出码 2 退出，并向标准错误（stderr）打印一条 BLOCKED（已拦截）消息。
