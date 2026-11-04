# 10 — Быстрые механизмы

state: todo
blocked-by: 08
target: ai-dev-hub-v2 @ v2-10

## What

Частые и опасные ошибки останавливаются механизмом, а не текстом, и без ложных блоков на корпусе полигона:
heredoc и `node -e` с экранированием, правка `.env`, локфайлов и применённых миграций, ревьюер не на Opus,
роли агентов (tester пишет только тесты, ревьюер не пишет файлов), голый `git push`, «зелёный» вывод тестов
при упавших. Стоп-хук судит по факту доклада и `last-full.json`, замок — по исполняемой программе, а не по
строке. Конец сессии без wind-down оставляет заглушку заметки.

## How

- Размер: M (калибровочно). Поток H, волна 2.
- Касания: модули PreToolUse / PostToolUse / Stop / SessionEnd, `plugin/hooks/base-rules.json`,
  `plugin/skills/dispatch/scripts/guard.cjs`, `plugin/skills/checks/scripts/checks.cjs` (`last-full.json`),
  записи реестра; горячие точки: каркас хуков, `base-rules.json` (все deny плана — здесь, включая deny
  Bash-записи в `memory/lessons/**` для 11), `guard.cjs` (до 18).
- `cd`: по итогу P1 — настройка платформы (ставит `/hub:setup`, срез 19) и удаление правила, либо
  `updatedInput` (если P8 прошла), либо deny-once для субагентов.
- PostToolUse(Bash) по выводу тестов: vitest/jest «exit 0 при упавших или пропущенных» → подача блока.
- SessionEnd-заглушка — только если P7 прошла; иначе — проверка на следующем SessionStart.
- Пуш: deny голого `git push` из папки диспетчера; пуш — скриптом (срез 15).
- Каждый механизм — с записью реестра и сценарием полигона (H01–H03, H09, M09, M10).
- Prior art: `rules-guard.cjs` (режимы log/ask/deny), `stop.cjs` (done-gate), H01–H03 полигона.

## Acceptance

- [ ] Полигон: H01–H03 «ловит» 100 %, ложных блоков 0 на корпусе, обходов замка ≥ 4/5 (цели
      `acceptance.md` §1).
- [ ] Каждый новый deny — тест «вход → решение» и строка реестра.
- [ ] Бюджет механизмов в pre-commit не превышен.

## Verify

`node --test plugin/ && node polygon/run.cjs --level hook --only 'H0[1239]|H1[01]'`

## Evidence
