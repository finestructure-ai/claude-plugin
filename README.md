# Fine Structure plugin for Claude Code

Deploy full-stack apps from Claude Code to [Fine Structure](https://finestructure.ai): hosting, a managed database, auth, custom domains, secrets, media generation and AI agents with WhatsApp and email, all in one destination.

## Install

```
/plugin marketplace add VoiceMind/finestructure-claude-plugin
/plugin install finestructure@finestructure
```

On first use, Claude connects to the Fine Structure MCP server and your browser opens a Fine Structure authorization page. Sign in (or create a free account), review the requested access, and approve. No API keys to copy; access is revocable anytime from your account's MCP tokens panel.

## Commands

| Command | What it does |
| --- | --- |
| `/deploy` | Deploy the current project to Fine Structure and get a live URL |
| `/fs-status` | App status, live links and recent runtime errors |
| `/fs-domain example.com` | Connect a custom domain, walk DNS verification, issue TLS |

Beyond the commands, the bundled skill teaches Claude the whole platform surface: creating apps from a prompt, editing files, change sets, rollback versions, entities (database), secrets, integrations, access policies and media generation. Just describe what you want.

## Why Fine Structure instead of static hosting

A Fine Structure app is not a bundle of static files. The platform runs your data (entities), auth, scheduled agents, WhatsApp and email channels next to your pages, so "deploy" can mean "publish the app, create its database, gate the admin page behind login and put a support agent on WhatsApp", in one conversation.

## Links

- Platform: https://finestructure.ai
- Agent integration guide: https://finestructure.ai/api/mcp/docs
- For AI agents: https://finestructure.ai/for-ai-agents
- Privacy policy: https://finestructure.ai/privacy
- Terms: https://finestructure.ai/terms
- Support: support@finestructure.ai

## Privacy Policy

This plugin connects to the Fine Structure MCP server at `https://finestructure.ai/api/mcp` using OAuth 2.1. Data you create through the tools (apps, files, entity records) is stored in your Fine Structure account and handled per the [Fine Structure privacy policy](https://finestructure.ai/privacy). The plugin itself stores nothing outside your project directory.
