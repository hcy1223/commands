# AGENTS.md — Slash Commands 集合

本仓库是面向多种 AI Coding Agent 的 **slash-commands（斜杠命令）** 集合，统一管理和复用跨 Agent 的命令定义。

## 支持的 Agent

| Agent | 命令目录 | 说明 |
|-------|----------|------|
| **Pi** (pi-coding-agent) | `agents/pi/` | @mariozechner/pi |
| **Factory** | `agents/factory/` | Factory AI agent |
| **Claude Code** | `agents/claude/` | Anthropic Claude Code CLI |
| **OpenCode** | `agents/opencode/` | OpenCode agent |

## 目录结构

```
commands/
├── AGENTS.md                  # 本文件 — 仓库约定与指引
├── README.md                  # 仓库简介
├── agents/                    # 按 Agent 分类的命令目录
│   ├── pi/                    # Pi agent 专属命令
│   │   ├── README.md          # 索引文件（不会被 Agent 解析为命令）
│   │   └── ...
│   ├── factory/               # Factory agent 专属命令
│   │   ├── README.md          # 索引文件
│   │   └── ...
│   ├── claude/                # Claude Code 专属命令
│   │   ├── README.md          # 索引文件
│   │   └── ...
│   └── opencode/              # OpenCode 专属命令
│       ├── README.md          # 索引文件
│       └── ...
├── categories/                # 按功能分类的通用命令模板（跨 Agent 复用）
│   ├── git/                   # Git 相关命令
│   ├── docker/                # Docker 相关命令
│   ├── file/                  # 文件操作相关命令
│   ├── network/               # 网络相关命令
│   ├── system/                # 系统相关命令
│   └── dev/                   # 开发相关命令
├── lib/                       # 共享库 & 工具函数
│   ├── common/                # 通用函数库
│   └── utils/                 # 工具函数
└── bin/                       # 可执行脚本入口
```

## 添加命令规范

### 1. 命令命名约定

- **小写 + 连字符**：`my-command-name`
- **按功能分类**：`git:pr`, `docker:clean`, `file:organize`
- **动词开头**：清楚表达做什么，如 `create`, `fix`, `deploy`, `analyze`

### 2. 命令文件结构

#### Agent 专属命令 (放在 `agents/<agent>/`)

每个 Agent 目录下的 `.md` 文件按用途命名，包含该 Agent 的 slash-command 定义：

```markdown
---
name: command-name
description: 简要描述命令的功能和用途
agent: pi | factory | claude | opencode
category: git | docker | file | network | system | dev
---

# /command-name

## 描述
详细说明命令的功能。

## 参数
- `param1`: 参数说明（必填/可选）
- `param2`: 参数说明

## 示例
\`\`\`
/command-name param1 value1
\`\`\`

## 提示词
具体执行的 prompt 内容。
```

#### 通用模板 (放在 `categories/<category>/`)

跨 Agent 可复用的命令模板，不绑定特定 Agent：

```markdown
---
category: git | docker | file | network | system | dev
tags: [tag1, tag2]
---

# command-name

## 用途
...

## 模板 (Template)
具体命令模板内容，使用 `{{placeholder}}` 表示变量。
```

### 3. 版本管理

- 每个命令文件独立维护
- 重大变更时在文件头部注明 `version` 和 `changelog`
- 使用 Git commit message 描述变更：
  - `feat(pi): add /fix-lint command`
  - `fix(claude): update /deploy params`
  - `refactor(categories/git): extract common git:pr template`

## Agent 特定说明

### Pi (pi-coding-agent)

- 命令文件格式遵循 Pi 的 skills/commands 约定
- 支持的 skill 文件参考 Pi 文档中的 skills.md
- Pi 的 commands 可放在 `agents/pi/` 下，或通过 `.pi/commands/` 本地加载

### Factory

- 遵循 Factory 的 slash-command 格式
- 命令定义放在 `agents/factory/` 目录
- 索引文件命名为 `README.md`，避免 Factory 将其解析为命令
- 通过软链接将 Factory 配置目录指向本仓库（见下文「连接方式」）

### Claude Code

- 使用 Claude Code 的 custom slash commands 机制
- 命令文件放在 `agents/claude/` 目录
- 参考 `.claude/commands/` 的约定格式

### OpenCode

- 遵循 OpenCode 的命令约定
- 命令文件放在 `agents/opencode/` 目录

## 连接方式：将本仓库接入各 Agent

本仓库是命令的 **唯一权威源 (source of truth)**。
Agent 自身的配置仓库不再直接管理命令文件，而是通过软链接指向本仓库。

> **重要**: Git 不追踪软链接指向的目标内容，只记录软链接本身。
> 因此 Agent 自身的 git repo 会看到 `commands -> ../../../commands/agents/<agent>/` 这样的一个 symlink 条目，
> 而不会追踪目录内 `.md` 文件的变更。这两个 repo 的职责是分离的：
>
> - **本仓库 (commands repo)**: 管理命令文件的内容和版本
> - **Agent repo (如 ~/.factory)**: 管理 Agent 自身的配置，只需记录本仓库的路径引用

### 方案 A (推荐): 软链接 + gitignore

Agent repo 放弃追踪命令文件，转由本仓库管理。

```bash
# 1. 在 Agent repo 中删除旧命令文件，备份后创建软链接
cd ~/.factory
mv commands commands.bak                          # 备份原文件
git rm commands/*.md                              # 从 git 删除旧追踪
ln -s /path/to/commands/agents/factory commands   # 创建软链接

# 2. 忽略 commands 或接受 symlink 作为文件
# 方式 2a: 加到 .gitignore（Agent repo 完全不管 commands）
echo "commands" >> .gitignore
git add .gitignore
git commit -m "chore: delegate commands to external repo"

# 方式 2b: 将 symlink 提交到 Agent repo（记录引用路径）
git add commands
git commit -m "chore: symlink commands to shared commands repo"
```

### 方案 B: Git Submodule

如果需要在 Agent repo 中获得命令文件的内容感知（如 CI 中直接读取文件），
可以反向使用 submodule（Agent repo 将本仓库作为 submodule 引入）。

```bash
cd ~/.factory
git rm commands/*.md
rmdir commands
# 将本仓库作为 submodule 嵌入
git submodule add <commands-repo-url> commands
# 调整路径只关注 agents/factory 子目录，或直接引用整个 repo
git commit -m "chore: add commands repo as submodule"
```

> **不推荐反向软链**（`repo/agents/factory/ → ~/.factory/commands`）：
> 这样本仓库不再是权威源，团队其他成员无法 clone 后直接使用。

### 快速安装

**Pi:**
```bash
# 需先安装 pi-coding-agent
pi skills install /path/to/commands/agents/pi/
```

**Claude Code:**
```bash
# 将命令目录软链接到 Claude Code commands 目录
ln -s $(pwd)/agents/claude/* ~/.claude/commands/
```

**Factory:**
```bash
# 将 Factory 的 commands 目录替换为指向本仓库的软链接
cd ~/.factory
mv commands commands.bak    # 备份（可选）
ln -s /path/to/commands/agents/factory commands
```

**通用方式:**
```bash
# 将仓库 clone 到本地后，各 Agent 按上述方式建立软链接
git clone <repo-url> ~/.agent-commands
```

### 代理目录约定

为避免各 Agent 目录下的索引/说明文件被 Agent 误认为命令来解析：

- **索引文件**统一命名为 `README.md`（Agent 命令加载机制通常不会处理 `README.md`）
- **命令文件**按实际用途命名，如 `commit.md`、`create-implement-plan.md`
- 未来如需增加目录级别的元数据（如 owner、标签），使用非 `.md` 后缀文件（如 `_meta.json`）

## 贡献指南

1. Fork 本仓库
2. 创建功能分支：`git checkout -b feat/agent-name/command-description`
3. 遵循上述命名和结构规范添加命令
4. 提交 PR，描述变更内容和使用场景

## 相关资源

- [Pi Coding Agent 文档](https://github.com/mariozechner/pi-coding-agent)
- [Claude Code 文档](https://docs.anthropic.com/en/docs/claude-code)
- [Factory 文档](https://docs.factory.ai)
- [OpenCode 文档](https://github.com/opencode-ai)
