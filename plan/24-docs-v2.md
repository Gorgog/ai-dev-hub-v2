# 24 — Доки v2

state: todo
blocked-by: 16, 22, 23
target: ai-dev-hub-v2 @ v2-24

## What

README и `CLAUDE.md` хаба описывают v2 как она есть: механизмы из реестра, формат памяти, три фразы
управления памятью, аварийный выключатель, установка рядом с v1, откат строкой `enabledPlugins`. Решения
плана сведены в durable-доки; план после приёмки удаляется по протоколу workplan (`done`).

## How

- Размер: S (калибровочно). Один поток, волна 4.
- Касания: `README.md`, `CLAUDE.md`, `docs/` (durable-доки); горячие точки: `README.md`, `CLAUDE.md`.
- Решения срезов берутся из их `## Evidence` и записей реестра, не из пересказа.
- Prior art: README v1.

## Acceptance

- [ ] Каждый механизм реестра упомянут в README одной строкой; нет упоминаний убранного.
- [ ] `workplan doc` по каждому durable-доку чист.

## Verify

`workplan doc docs/hub-v2.md` (скрипт `bin/workplan.mjs` плагина плана работ)

## Evidence
