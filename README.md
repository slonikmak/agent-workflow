# gh-workflow

Шаблон **plan-first** процесса для разработки с автономными агентами в GitHub.

Идея простая: **план и спецификация — до кода**, а изменения в `src/` и `tests/` запрещены до ручного апрува плана (`plan:approved`).

## Что внутри

- `.github_/workflows/`
  - `block-code-before-plan-approved.yml` — блокирует PR, если меняются `src/`/`tests/` без лейбла `plan:approved`.
  - `enforce-pr-body-why-plan-spec-fixes.yml` — требует в PR body поля `Fixes`, `Spec`, `Plan`, `Why` (для PR, которые трогают `src/`/`tests/`).
- `.agents_/skills/` — “скиллы” (команды) агента для работы по этому процессу: `capture`, `plan`, `execute`, `replan`, `report`, `review`.
- `docs/` — описание процесса, форматов документов и примеры.
  - Начни с `docs/AGENT_WORKFLOWS.md` и `docs/DOCUMENT_FORMATS.md`.
  - `docs/raw/` — черновики/исходники заметок.

## Как подключить к своему репозиторию

> В этом репозитории папки названы с подчёркиванием (`.github_`, `.agents_`), чтобы ничего не включалось случайно. Для использования их нужно перенести/переименовать.

1) **GitHub Actions (enforcement)**

- Скопируй файлы из `.github_/workflows/` в `.github/workflows/` целевого репозитория
  **или** переименуй `.github_` → `.github`.

2) **Agent skills (опционально)**

- Скопируй `.agents_/skills/` в `.agents/skills/` целевого репозитория
  **или** переименуй `.agents_` → `.agents`.

3) **Заведи лейблы (как минимум)**

- `status:backlog`
- `status:todo`
- `status:in-progress`
- `plan:approved`

## Минимальный контракт PR (когда меняется код/тесты)

PR description должна содержать:

```text
Fixes #123
Spec: R-013
Plan: tasks/T-0123.md

Why:
- ...
```

## Документы процесса

- `docs/AGENT_WORKFLOWS.md` — конституция и сценарии (FSM/лейблы).
- `docs/AGENT_INSTRUCTION_PLAN.md` — правила и шаблон для `tasks/T-XXXX.md`.
- `docs/AGENT_INSTRUCTION_SPEC.md` — правила ведения `spec.md` (RFC 2119 + R-XXX).
- `docs/DOCUMENT_FORMATS.md` — краткая памятка по формату Spec/Task.

## Настройка под свой репозиторий

- Если у вас другой layout (нет `src/` или `tests/`), обнови `CODE_PATH_PREFIXES` в workflow-файлах.
- Если хотите исключать некоторые PR по типу (`docs(...)`, `chore(...)`, `spec(...)`), настрой `EXEMPT_KINDS` в `enforce-pr-body-why-plan-spec-fixes.yml`.
