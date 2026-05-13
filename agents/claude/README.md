# Claude Code Commands

Claude Code (Anthropic) 的 slash-commands 集合。

> 命令文件遵循 `.claude/commands/` 原生格式，可直接软链接到 `~/.claude/commands/` 或项目 `.claude/commands/` 目录使用。

## 命令列表

| 命令 | 文件 | 说明 |
|------|------|------|
| `/commit` | [commit.md](commit.md) | 分析变更并创建 git commit（自动生成 Conventional Commits 消息） |
| `/commit-push-pr` | [commit-push-pr.md](commit-push-pr.md) | 一键提交 + 推送 + 创建 PR（完整工作流） |
| `/clean-gone` | [clean-gone.md](clean-gone.md) | 清理远程已删除的本地分支及关联 worktree |

## 使用方式

```bash
# 开发过程中快速提交
/commit

# 功能完成，一键推送到 PR
/commit-push-pr

# 定期清理已合并的分支
/clean-gone
```

## 安装

```bash
# 方案 A: 软链接整个目录到 Claude Code 项目 commands
ln -s $(pwd)/agents/claude/commit.md .claude/commands/commit.md
ln -s $(pwd)/agents/claude/commit-push-pr.md .claude/commands/commit-push-pr.md
ln -s $(pwd)/agents/claude/clean-gone.md .claude/commands/clean-gone.md

# 方案 B: 软链接到全局 Claude Code commands
ln -s $(pwd)/agents/claude/commit.md ~/.claude/commands/commit.md
ln -s $(pwd)/agents/claude/commit-push-pr.md ~/.claude/commands/commit-push-pr.md
ln -s $(pwd)/agents/claude/clean-gone.md ~/.claude/commands/clean-gone.md

# 方案 C: 直接复制
cp agents/claude/*.md .claude/commands/
```

## 参考来源

命令内容参考自 Anthropic 官方 [commit-commands](https://github.com/anthropics/claude-code/tree/main/plugins/commit-commands) 插件。
