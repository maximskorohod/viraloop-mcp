# Шпаргалка по инструментам viraloop

Источник истины: схемы инструментов, которые отдаёт сервер (tools/list). Права: read, edit, spend.

| Инструмент | Когда | Ключевые поля | Типичные ошибки |
|---|---|---|---|
| `create_upload` | новый ролик, нужна ссылка загрузки | вход: `title` (до 120 знаков); выход: `upload_url`, `expires_at` | нет прав edit |
| `start_job` | запуск нарезки после «да» на цену | вход: `asset_id`, `idempotency_key`, `style`; выход: задание с `id`, `status`, `plan_id` | `insufficient_credits`, `idempotency_conflict`, `unknown_style`, `not_found` |
| `get_job` | опрос статуса, ссылка на скачивание | `status` (queued, running, done, failed, dead, canceled), `error`, `error_code`, `plan_id`, `render_id`, `result`, `download_url` | `not_found` |
| `get_plan` | показать «что вырезано», взять `revision` | вход: `plan_id`, `variant`; выход: `revision`, `removed[]` (`n`, `reason`, `t0_ms`, `t1_ms`, `word_ids`, `text`, `restorable`, `restore`) | `not_found` |
| `revise` | вернуть фразу, сменить стиль, вариант, слой | вход: `plan_id`, `base_revision`, `variant`, `ops[]`; выход: новая ревизия, `render_id`, `job_id`, `rerender_paid`, `rerender_credits`, `free_rerenders_left` | `revision_conflict`, `cut_cache_missing`, `bad_ops`, `insufficient_credits`, `unknown_style` |
| `get_timeline` | структура ролика для предпросмотра | вход: `plan_id`, `variant`, `revision`; выход: segments, captions, layers, removed | `not_found`, `cut_cache_missing` |
| `cancel` | остановить задание | вход: `job_id`; выход: `id`, `status` | `not_found` |
| `get_balance` | перед платным шагом, вопрос про кредиты | `balance`, `reserved`, `available`, `subscription` | нет |
| `list_projects` | пользователь потерял `plan_id` | вход: `limit`; выход: `projects[]` | нет |

## ops для revise

| op | поля | смысл |
|---|---|---|
| `restore_words` | `word_ids` | вернуть вырезанные слова (фразу) |
| `restore_range` | `src_t0_ms`, `src_t1_ms` | вернуть паузу (до 30 с, внутри вырезанного); готовая операция лежит в `removed[].restore` |
| `set_style` | `style` | сменить стиль |
| `add_layer` | `layer` (layer_id, kind effect/gen/caption/audio, ref, src_t0_ms, src_t1_ms, z, blend, props, fit, word_ids) | добавить эффект или слой |
| `remove_layer` | `layer_id` | убрать слой |

## Не подключены на сервере

`estimate_gen`, `run_gen` (генерация только в кабинете), `analyze_reel`, `suggest_format` (после беты).
Тренды подключены: `search_trends`, `list_trend_words`, `release_trend_word`.

## Что платно

- `start_job`: резерв по исходнику (до `reel_max_s` с = `reel_credits`, длиннее ceil(минуты x `long_per_minute`)); при `done` списывается цена готового ролика, остаток и весь резерв при `dead` и отмене возвращаются.
- `revise`: первые 3 перерендера на план бесплатно, дальше `rerender_credits` за каждый.
- Чтение (`get_*`, `list_*`) бесплатно, с лимитом частоты.
