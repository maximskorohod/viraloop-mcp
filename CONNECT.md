# Как подключить Viraloop к своей нейросети

Адрес один для всех: `https://api.getviraloop.com/mcp`. Вход через OAuth: приложение само откроет страницу Viraloop,
вы входите кодом из письма и отмечаете, что ему можно. Ключ из кабинета нужен только там, где OAuth нет.

## Что можно разрешить

| Право | Что даёт |
|---|---|
| Смотреть (всегда) | проекты, планы монтажа, баланс, каталоги стилей и эффектов |
| Править | ссылка на загрузку, правки плана без оплаты, отмена |
| Тратить кредиты | запуск монтажа и платные пересборки, не больше дневного лимита, который вы задали |

Отозвать доступ: кабинет, раздел «ИИ», кнопка «Отозвать» у приложения. Доступ пропадает сразу.

## Claude Code
Плагин ставит и правила, и подключение:
```
/plugin marketplace add maximskorohod/viraloop-mcp
/plugin install viraloop@viraloop
```
Или только сервер: `claude mcp add --transport http viraloop https://api.getviraloop.com/mcp`.
Затем команда `/mcp`, пункт viraloop, «Authenticate».

## Claude (claude.ai, Desktop, телефон)
1. Настройки, «Коннекторы», «Добавить свой коннектор».
2. Название `Viraloop`, адрес `https://api.getviraloop.com/mcp`.
3. «Подключить»: откроется страница Viraloop, войдите кодом из письма и нажмите «Разрешить».
Скилл с правилами работы: папка `claude/skills/viraloop` zip-архивом в Настройки, Возможности, Скиллы.

## ChatGPT
1. Настройки, «Приложения и коннекторы», «Дополнительно», включить режим разработчика.
2. «Создать»: адрес `https://api.getviraloop.com/mcp`, вход OAuth.
3. Войдите кодом из письма и разрешите доступ. Инструкции для проекта: `chatgpt/instructions.md`.

## Cursor
В `~/.cursor/mcp.json` (или `.cursor/mcp.json` проекта):
```json
{ "mcpServers": { "viraloop": { "url": "https://api.getviraloop.com/mcp" } } }
```
В настройках Cursor, раздел MCP, у viraloop нажать «Login». Правила для агента: `cursor/viraloop.mdc` в `.cursor/rules/`.

## Codex CLI
```
codex mcp add viraloop --url https://api.getviraloop.com/mcp
codex mcp login viraloop
```
Правила: `codex/AGENTS.md` в корень проекта.

## Gemini CLI
```
gemini extensions install https://github.com/maximskorohod/viraloop-mcp --path gemini/viraloop
```
или в `~/.gemini/settings.json`: `"mcpServers": { "viraloop": { "httpUrl": "https://api.getviraloop.com/mcp" } }`.
При первом вызове Gemini CLI откроет вход сам (или команда `/mcp auth viraloop`).

## Hermes Agent
Подробно: `hermes/README.md`. Коротко, в `~/.hermes/config.yaml`:
```yaml
mcp_servers:
  viraloop:
    url: https://api.getviraloop.com/mcp
    auth: oauth
```
Затем `hermes mcp login viraloop`.

## Если не подключается
- «Запрос устарел»: страница входа живёт 15 минут, начните подключение заново.
- Приложение просит client_id: оставьте пустым, Viraloop регистрирует приложения сам.
- Ключ засветился: отзовите его в кабинете, выпустите новый.
