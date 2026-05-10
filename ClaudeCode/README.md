---
created: 2026-05-10 05:39
modified: 2026-05-10 05:51
tags:
  - Claude
  - Knowledge
  - Claude/Installation
source:
---
> [!quote] The only person who never makes mistakes is the person who never does anything.
> — Denis Waitley

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**

---

For starting to learn claudeCode, go to this page [Home](Home.md) and you will find everything you need to get started.

Claude Code is an AI coding agent from Anthropic. Normally, you use it with a Claude subscription or Anthropic API billing, but you can still learn the Claude Code workflow by routing it through [OpenRouter](https://openrouter.ai/) and using free OpenRouter models.

> [!warning]
> This does **not** mean you get paid Claude models for free. You get the Claude Code CLI/workflow, but the actual model can be a free OpenRouter model. Free models have low rate limits and may be weaker than Claude.

Useful links:

| Need | Link |
|---|---|
| Official Claude Code setup | [Claude Code setup docs](https://code.claude.com/docs/en/setup) |
| Claude Code environment variables | [Claude Code env vars](https://code.claude.com/docs/en/env-vars) |
| OpenRouter + Claude Code guide | [OpenRouter Claude Code integration](https://openrouter.ai/docs/cookbook/coding-agents/claude-code-integration) |
| Create OpenRouter API key | [OpenRouter API keys](https://openrouter.ai/settings/keys) |
| Free OpenRouter router | [openrouter/free](https://openrouter.ai/openrouter/free) |
| OpenRouter FAQ and free limits | [OpenRouter FAQ](https://openrouter.ai/docs/faq) |

## What This Setup Gives You

You can use the `claude` command, learn Claude Code commands, practice agent workflows, and understand how Claude Code works inside projects.

But the model is not necessarily Claude. If you choose `openrouter/free`, OpenRouter automatically routes requests to available free models.

> [!note]
> OpenRouter says Claude Code with OpenRouter is only guaranteed to work with Anthropic first-party providers. Other free models may work for learning, but they can fail or behave differently because Claude Code is optimized for Anthropic models.

## Step 1: Create an OpenRouter API Key

1. Go to [OpenRouter API keys](https://openrouter.ai/settings/keys).
2. Create a new key.
3. Copy it immediately and save it somewhere private.

> [!warning]
> Never paste your API key into a public note, GitHub repo, screenshot, or shared chat.

## Step 2: Install Claude Code

Official install docs: [Claude Code setup](https://code.claude.com/docs/en/setup)

### macOS, Linux, or WSL

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

### Windows PowerShell

```powershell
irm https://claude.ai/install.ps1 | iex
```

### npm alternative

Requires Node.js 18+.

```bash
npm install -g @anthropic-ai/claude-code
```

## Step 3: Configure Claude Code to Use OpenRouter

Create or edit this file:

```text
~/.claude/settings.json
```

Add this:

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://openrouter.ai/api",
    "ANTHROPIC_AUTH_TOKEN": "sk-or-v1-your-openrouter-key-here",
    "ANTHROPIC_API_KEY": "",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "openrouter/free",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "openrouter/free",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "openrouter/free",
    "CLAUDE_CODE_SUBAGENT_MODEL": "openrouter/free"
  },
  "effortLevel": "xhigh",
  "theme": "dark-ansi"
}
```

> [!tip]
> You can replace `openrouter/free` with a specific free model from OpenRouter if you want more control. Free model IDs usually end with `:free`.

## Step 4: Start and Verify

Open a project folder, then run:

```bash
claude
```

Inside Claude Code, run:

```text
/status
```

You should see:

```text
Auth token: ANTHROPIC_AUTH_TOKEN
Anthropic base URL: https://openrouter.ai/api
```

You can also check the [OpenRouter activity dashboard](https://openrouter.ai/activity) to confirm requests are going through OpenRouter.

## Free Limits

OpenRouter says free models are limited:

| Account state | Free model limit |
|---|---:|
| No purchased credits | 50 free model requests/day |
| At least $10 purchased credits | 1000 free model requests/day |

So this is enough for learning Claude Code, testing commands, and understanding the workflow, but it is not a replacement for a real paid Claude setup.

## Key Takeaway

At the end, you will have Claude Code running through OpenRouter. You will be learning the Claude Code agent workflow, but you are not getting Claude models for free. This is still useful because Claude Code is one of the important coding agents right now, so learning the workflow is worth it.
