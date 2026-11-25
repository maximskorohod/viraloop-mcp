# Viraloop for DeepSeek Harness

Plugin `dsh-viraloop`: connects the hosted Viraloop MCP server (`https://api.getviraloop.com/mcp`) to DeepSeek Harness
through its built-in MCP client. Tools show up as `mcp__viraloop__<tool>`.

## Install

1. In the Viraloop dashboard, "AI" tab, create a key (choose permissions and a daily credit limit).
2. Save it in DeepSeek Harness as the credential `VIRALOOP_MCP_TOKEN` (Settings, credentials),
   or add `VIRALOOP_MCP_TOKEN=<key>` to `~/.dsh/.env`. Never paste the key into the chat.
3. Add the plugin to your profile:
   ```
   dsh plugin --profile desktop add github:maximskorohod/viraloop-mcp#path:/deepseek
   ```
   then restart DeepSeek Harness.
4. Rules for the agent: copy `skills/viraloop` from this folder to `~/.agents/skills/viraloop`
   (DSH reads skills from there; Codex and other agents read the same folder).

Check: ask the agent "show my Viraloop balance".

## Русский

1. Кабинет Viraloop, «ИИ», «Новый ключ» (права и дневной лимит).
2. В DeepSeek Harness сохранить его как секрет `VIRALOOP_MCP_TOKEN` (Настройки, ключи) или строкой
   `VIRALOOP_MCP_TOKEN=<ключ>` в `~/.dsh/.env`. В чат ключ не вставлять.
3. `dsh plugin --profile desktop add github:maximskorohod/viraloop-mcp#path:/deepseek`, перезапустить DSH.
4. Правила: папку `skills/viraloop` скопировать в `~/.agents/skills/viraloop`.

Why a key and not OAuth: the DSH MCP client sends static headers only; it has no OAuth flow yet.
