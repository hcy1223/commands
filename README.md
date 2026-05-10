# Slash Commands 集合

面向多种 AI Coding Agent 的统一 slash-commands 集合。

## 支持的 Agent

- **Pi** — [pi-coding-agent](https://github.com/mariozechner/pi-coding-agent)
- **Factory** — Factory AI agent
- **Claude Code** — [Anthropic Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- **OpenCode** — OpenCode agent

## 目录结构

```
commands/
├── agents/         # 按 Agent 分类的命令定义
│   ├── pi/
│   ├── factory/
│   ├── claude/
│   └── opencode/
├── categories/     # 按功能分类的通用命令模板
│   ├── git/
│   ├── docker/
│   ├── file/
│   ├── network/
│   ├── system/
│   └── dev/
├── lib/            # 共享库 & 工具
├── bin/            # 可执行脚本
└── AGENTS.md       # 仓库约定与开发指引
```

## 约定

请阅读 [AGENTS.md](./AGENTS.md) 了解命令命名、文件结构和贡献规范。
