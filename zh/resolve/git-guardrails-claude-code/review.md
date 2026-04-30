## git-guardrails-claude-code：给 Claude Code 装上 Git 安全阀

这个技能解决一个具体而真实的痛点：Claude Code 在自主执行任务时，可能会运行 `git push --force` 或 `git reset --hard` 这类破坏性命令，而这些操作一旦执行就难以撤销。`git-guardrails-claude-code` 通过 Claude Code 的 PreToolUse Hook 机制，在这些命令真正执行之前将其拦截。

### 技能定位

它是一个**一次性安装型**技能——运行一次，配置好 Hook，之后永久生效，不需要每次会话重新激活。目标用户是担心 AI 误操作 Git 仓库的开发者，尤其是在把 Claude Code 用于自动化任务（AFK 模式）时，没有人在旁边盯着每一条命令。

### 核心设计哲学：在工具层拦截，而非在提示层约束

这是这个技能最关键的设计决策。它没有选择"在系统提示里告诉 Claude 不要 push"，而是在 Claude Code 的工具调用层设置硬性拦截——Claude 根本没有机会执行这些命令，而不是"被告知不应该执行"。

两种方式的区别是本质性的：提示层约束依赖模型的遵从性，在复杂任务中可能被覆盖或遗忘；工具层拦截是确定性的，无论上下文如何，命令都会被阻断。这是"防御性编程"思维在 AI 工具链中的直接应用。

### 关键机制细节

**PreToolUse Hook** 是 Claude Code 提供的扩展点：在任何工具调用执行之前，先运行指定的 Shell 脚本。脚本通过 stdin 接收工具调用的 JSON 数据（包含 `tool_input.command` 字段），通过退出码决定是否放行——退出码 2 表示阻断，Claude 会收到一条"无权执行此命令"的消息。

**拦截列表**的选择体现了对"破坏性"的精确定义：
- `git push`（含 `--force`）：影响远程仓库，他人可见
- `git reset --hard`：丢弃本地未提交修改，不可恢复
- `git clean -f/-fd`：删除未跟踪文件，不可恢复
- `git branch -D`：强制删除分支，可能丢失提交
- `git checkout .` / `git restore .`：覆盖工作区修改，不可恢复

这些命令的共同特征是：**执行后无法通过 Git 本身撤销**（或撤销代价极高）。普通的 `git commit`、`git add`、`git stash` 不在列表中，因为它们是可逆的。

**作用域二选一**（项目级 vs 全局）是一个务实的设计：项目级配置写入 `.claude/settings.json`，只影响当前仓库；全局配置写入 `~/.claude/settings.json`，影响所有项目。技能在安装前先询问用户偏好，而不是自作主张选择其中一个。

**合并而非覆盖**的 settings 写入策略防止了破坏已有配置：如果 settings 文件已存在，新的 Hook 合并进 `hooks.PreToolUse` 数组，不覆盖其他设置。

**可验证的安装测试**是这个技能的收尾动作：用一条 echo 命令模拟 Claude Code 的 Hook 调用，验证脚本确实能拦截 `git push`。这把"我以为装好了"变成了"我确认装好了"。

### 亮点总结

`git-guardrails-claude-code` 的精妙之处在于它选择了**正确的拦截层**。在 AI 自主化程度越来越高的今天，"告诉 AI 不要做某事"的可靠性远低于"让 AI 物理上无法做某事"。这个技能把一个本来需要手动配置的安全措施，包装成了一个五步安装流程，让开发者在把 Claude Code 放出去自主工作之前，能够快速建立一道确定性的安全护栏。
