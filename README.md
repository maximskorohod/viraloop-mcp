# Viraloop MCP

Edit talking-head reels from your AI agent. Viraloop cuts pauses, filler words and retakes, lets you restore a phrase,
switch styles and download the finished vertical video. This repo holds the agent skills and setup guides;
the server is hosted at:

```
https://api.getviraloop.com/mcp
```

Sign-in is OAuth 2.1: your client opens a Viraloop page, you log in with a code sent to your email and choose what the
agent may do (view, edit, spend credits within a daily limit). Revoke access any time in the Viraloop dashboard, "AI" tab.

## Quick start

This repo is one plugin (skill + server connection) that Claude Code, Codex, Hermes, Cursor, Gemini CLI and Kimi Code
all read, plus a DeepSeek Harness package in `deepseek/`.

**Claude Code**
```
/plugin marketplace add maximskorohod/viraloop-mcp
/plugin install viraloop@viraloop
```
Then `/mcp`, pick `viraloop`, "Authenticate".

**Codex** (CLI and app)
```
codex plugin marketplace add maximskorohod/viraloop-mcp
codex plugin add viraloop@viraloop
codex mcp login viraloop
```

**Hermes Agent**
```
hermes plugins install maximskorohod/viraloop-mcp --enable
```
Sign in on first use. Details and a key-based fallback: `hermes/README.md`.

**Gemini CLI**
```
gemini extensions install https://github.com/maximskorohod/viraloop-mcp
```

**Kimi Code** (in the TUI)
```
/plugins install https://github.com/maximskorohod/viraloop-mcp
/reload
/mcp-config login viraloop
```

**DeepSeek Harness**: key-based (its MCP client has no OAuth yet), see `deepseek/README.md`:
```
dsh plugin --profile desktop add github:maximskorohod/viraloop-mcp#path:/deepseek
```

**Cursor**: add to `~/.cursor/mcp.json`:
```json
{ "mcpServers": { "viraloop": { "url": "https://api.getviraloop.com/mcp" } } }
```
then "Login" next to viraloop in Settings, MCP. Rules: copy `cursor/viraloop.mdc` to `.cursor/rules/`.

**Claude.ai / Claude Desktop**: Settings, Connectors, "Add custom connector", URL above.
Optional: upload `skills/viraloop` as a zip in Settings, Capabilities, Skills.

**ChatGPT**: Settings, Apps & Connectors, Advanced, Developer mode, Create: URL above, OAuth.
Instructions for a project: `chatgpt/instructions.md`.

Full guide per client: [CONNECT.md](CONNECT.md).

## What the agent can do

Create an upload link, start an edit, show what was cut and why, restore words or pauses, apply a style preset,
re-render, export the MP4, check balance and prices, search trending reels by keyword. Paid actions need the `spend`
permission, stay within your daily limit, and the skills tell the agent to name the price and ask before running them.

## Русский

Viraloop монтирует рилсы «говорящей головы» по команде вашей нейросети: режет паузы, слова-паразиты и дубли, возвращает
фразу, меняет стиль, отдаёт готовый ролик. Подключение по одной ссылке `https://api.getviraloop.com/mcp`, вход кодом
из письма, права и дневной лимит выбираете сами. Инструкция по каждому клиенту: [CONNECT.md](CONNECT.md).

## License

Skill texts and guides: MIT. The Viraloop service itself is not open source.
