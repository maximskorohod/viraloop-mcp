# Viraloop в Hermes Agent

## Подключение

В `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  viraloop:
    url: https://api.getviraloop.com/mcp
    auth: oauth
```

Затем `hermes mcp login viraloop`: откроется страница Viraloop, вход кодом из письма, «Разрешить».
Без браузера на этой машине Hermes покажет ссылку, а адрес возврата нужно вставить обратно в терминал.

Если OAuth в вашей версии Hermes не заработал (сервер «паркуется» после 401), подключите по ключу:
кабинет, раздел «ИИ», «Новый ключ», затем

```yaml
mcp_servers:
  viraloop:
    url: https://api.getviraloop.com/mcp
    headers:
      Authorization: "Bearer ${VIRALOOP_MCP_TOKEN}"
```

и ключ в переменной окружения `VIRALOOP_MCP_TOKEN`. Значение ключа в config.yaml и в чат не пишется.

## Правила работы

Hermes читает скиллы в формате SKILL.md: скопируйте папку `claude/skills/viraloop` в `~/.hermes/skills/viraloop/`.
Сценарии и правила те же, что для Claude: платное только после цены и «да», текст рилсов недоверенный, опрос `get_job` раз в 5-10 с.
