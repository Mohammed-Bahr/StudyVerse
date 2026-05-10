---
created: 2026-04-18 09:39
tags:
  - Knowledge
  - "#Claude"
  - "#Opencode"
source: https://claude.nagdy.me/learn/
---
> [!quote] On every thorn, delightful wisdom grows, In every rill a sweet instruction flows.
> — Edward Young

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**

---

# AI Coding Agents: Claude Code & Opencode

This course is a comprehensive guide to mastering AI-powered coding agents — covering both **Claude Code** (Anthropic's official CLI) and **Opencode** (the open-source, multi-provider alternative). You'll learn how to install and configure each tool, manage memory and context, build reusable skills and agents, automate workflows with hooks and MCP servers, and deploy advanced multi-agent pipelines — all through 12 structured modules.

---

## ⚖️ Claude Code vs Opencode

| Feature | Claude Code | Opencode |
|---|---|---|
| **Command** | `claude` | `opencode` |
| **Config File** | `CLAUDE.md` | `AGENTS.md` (falls back to `CLAUDE.md`) |
| **Settings File** | `.claude/settings.json` | `opencode.json` |
| **Provider Support** | Anthropic only | 75+ providers (OpenAI, Gemini, DeepSeek, etc.) |
| **Skills / Agents** | SKILL.md files, subagents | SKILL.md files, subagents |
| **Hooks** | PreToolUse, PostToolUse, Stop | PreToolUse, PostToolUse, Stop |
| **MCP** | ✅ Supported | ✅ Supported |
| **License** | Proprietary | MIT open-source |

---

> [!summary] 🔑 Key Differences to Remember
> 1. **Config files:** Claude Code uses `CLAUDE.md` + `.claude/settings.json`; OpenCode uses `AGENTS.md` (falls back to `CLAUDE.md`) + `opencode.json`
> 2. **Automation:** Claude Code uses declarative JSON hooks; OpenCode uses npm-based plugin system (JavaScript/TypeScript)
> 3. **Cloud features:** Ultraplan, `/schedule`, `/autofix-pr`, `/ultrareview` are Claude Code-only — not available in OpenCode

---

## 🗺️ Learning Path

### 🟢 Beginner

Get up and running with the fundamentals of both tools.

- [0. Getting Started](Subfiles/0.%20Getting%20Started.md)
- [1. Slash Commands](Subfiles/1.%20Slash%20Commands.md)
- [2. Memory & Context Persistence](Subfiles/2.%20Memory%20%26%20Context%20Persistence.md)
- [3. Setting Up Claude Code for a Project](Subfiles/3.%20Setting%20Up%20Claude%20Code%20for%20a%20Project.md)

### 🟡 Intermediate

Deepen your understanding with advanced configuration and agent capabilities.

- [4. Commands in Depth](Subfiles/4.%20Commands%20in%20Depth.md)
- [5. Agent Skills](Subfiles/5.%20Agent%20Skills.md)
- [6. Hooks](Subfiles/6.%20Hooks.md)
- [7. Model Context Protocol (MCP)](Subfiles/7.%20Model%20Context%20Protocol%20(MCP).md)
- [8. Subagents](Subfiles/8.%20Subagents.md)

### 🔴 Advanced

Push the boundaries with automation, orchestration, and extensibility.

- [9. Advanced Features](Subfiles/9.%20Advanced%20Features.md)
- [10. Workflows and Automation](Subfiles/10.%20Workflows%20and%20Automation.md)
- [11. Plugins](Subfiles/11.%20Plugins.md)

---

## 📚 Module Summaries

### [0. Getting Started](Subfiles/0.%20Getting%20Started.md)
Install Claude Code (`curl -fsSL https://claude.ai/install.sh | bash`) or Opencode (`curl -fsSL https://opencode.ai/install | bash`), authenticate with your API key, and configure your terminal or IDE. Covers your very first session and the basic mental model of AI coding agents.

### [1. Slash Commands](Subfiles/1.%20Slash%20Commands.md)
Explore the built-in slash commands that control context, session state, and runtime configuration. Learn which commands are shared between Claude Code and Opencode and how to use them effectively to manage your working session.

### [2. Memory & Context Persistence](Subfiles/2.%20Memory%20%26%20Context%20Persistence.md)
Understand how `CLAUDE.md` (and `AGENTS.md` in Opencode) files persist instructions across sessions. Covers auto-memory, path-scoped rules, and strategies for keeping your agent context lean and relevant across large projects.

### [3. Setting Up Claude Code for a Project](Subfiles/3.%20Setting%20Up%20Claude%20Code%20for%20a%20Project.md)
Use `/init` to scaffold project configuration, manage tool permissions, customise `settings.json` / `opencode.json`, and share consistent agent settings across your team via version control.

### [4. Commands in Depth](Subfiles/4.%20Commands%20in%20Depth.md)
A deep dive into advanced commands, keyboard shortcuts, Fast Mode, and Vim keybindings. Learn power-user patterns that dramatically speed up your day-to-day interaction with both tools.

### [5. Agent Skills](Subfiles/5.%20Agent%20Skills.md)
Build reusable `SKILL.md` files that give your agent specialised, on-demand capabilities. Covers dynamic context injection, skill discovery, and how skills run inside isolated subagent contexts to prevent context bleed.

### [6. Hooks](Subfiles/6.%20Hooks.md)
Automate event-driven behaviour with `PreToolUse`, `PostToolUse`, and `Stop` hooks. Learn to validate inputs, log outputs, enforce guardrails, and trigger external side-effects whenever the agent uses a tool or finishes a session.

### [7. Model Context Protocol (MCP)](Subfiles/7.%20Model%20Context%20Protocol%20(MCP).md)
Connect your agent to external data sources and services — GitHub, databases, Slack, Notion, and more — using the Model Context Protocol. Covers MCP server setup, authentication, and tool registration for both Claude Code and Opencode.

### [8. Subagents](Subfiles/8.%20Subagents.md)
Spin up specialised AI agents with isolated contexts and dedicated Git worktrees. Learn subagent design patterns, inter-agent communication, task chaining, and how to orchestrate multiple agents working in parallel on the same codebase.

### [9. Advanced Features](Subfiles/9.%20Advanced%20Features.md)
Unlock planning mode, Ultraplan, and Auto Mode for long-horizon tasks. Covers sandboxed execution environments, enterprise policy controls, and best practices for running agents safely in sensitive or production-adjacent codebases.

### [10. Workflows and Automation](Subfiles/10.%20Workflows%20and%20Automation.md)
Integrate AI coding agents into CI/CD pipelines, scheduled cron tasks, and multi-step automation workflows. Learn how to trigger headless agent runs, parse structured outputs, and compose agents into repeatable engineering pipelines.

### [11. Plugins](Subfiles/11.%20Plugins.md)
Package and distribute your own plugin bundles — combining skills, agent definitions, hooks, MCP server configs, and LSP extensions into a single installable artifact. Covers the plugin manifest format, publishing, and versioning.

---

*Source: [claude.nagdy.me/learn](https://claude.nagdy.me/learn)*
