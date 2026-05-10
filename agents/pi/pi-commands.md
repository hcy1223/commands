# Pi Agent Commands

Pi (pi-coding-agent) 的 prompt templates 集合。

> Pi 中 **Prompt Templates** 最接近 Factory 的 slash commands。
> 文件名为命令名，通过 `/name` 调用，支持 `$ARGUMENTS` 变量。

## 命令列表

| 命令 | 文件 | 说明 |
|------|------|------|
| `/commit` | `commit.md` | 仅提交已暂存的变更，自动规范化 Conventional Commits 消息 |
| `/add-commit` | `add-commit.md` | 暂存所有文件后提交，等于 `git add -A` + `/commit` |

## 使用方式

```bash
# 先暂存，再提交
git add src/
/commit feat(api): add user endpoint

# 一键暂存+提交
/add-commit fix: correct login validation
```

## 安装

```bash
# 软链接到 Pi prompts 目录
ln -s $(pwd)/agents/pi/commit.md ~/.pi/agent/prompts/commit.md
ln -s $(pwd)/agents/pi/add-commit.md ~/.pi/agent/prompts/add-commit.md

# 或软链接整个目录
ln -s $(pwd)/agents/pi/*.md ~/.pi/agent/prompts/
```
