# Исследование 01: структура и ключевые файлы agent-first проектов

> **Статус:** исследовательский отчет, не утвержденная методология.
>
> **Срез:** 2026-10-17.
>
> **Решения в этом документе:** рабочие рекомендации для обсуждения. Они не
> становятся обязательными для других проектов до отдельного согласования.

## 1. Краткий вывод

К октябрю 2026 года сообщество не пришло к единой полной структуре
репозитория для AI-first разработки. Однако уже сложился достаточно четкий
набор совместимых принципов:

1. **Короткий корневой `AGENTS.md`** используется как переносимая точка входа
   и карта проекта, а не как энциклопедия.
2. **Подробное знание живет в структурированном `docs/`**, рядом с кодом и под
   машинной проверкой.
3. **Инструкции, workflow и память разделяются по назначению:** постоянные
   правила, path-scoped rules, skills, планы, состояние, решения и артефакты
   выполнения не должны превращаться в один большой Markdown-файл.
4. **GitHub Issue является единицей работы**, а GitHub Projects —
   операционной проекцией очереди и состояния. Project не должен дублироваться
   отдельным master-`TODO.md`.
5. **План нужен не для каждой карточки**, а для длинной или рискованной работы.
   Для нее план связывается с Issue, а не становится вторым backlog.
6. **Файл состояния нужен как handoff-пакет**, но его сведения должны быть
   указателями и кратким контекстом. Текущий статус задач должен повторно
   проверяться в GitHub, а не безоговорочно приниматься из старого файла.
7. **Произвольные инструкции агента не являются enforcement.** Надежность
   обеспечивается CI, тестами, линтерами, hooks, permissions, sandbox,
   branch/ruleset policy и автоматическим обновлением Project.
8. **Для полностью агентной разработки важнее agent legibility, чем удобство
   человеческого чтения исходного кода:** агент должен быстро понять, что
   делать, как проверить результат, где зафиксировать состояние и когда
   остановиться.

Для вашего режима работы оптимальна не одна жесткая структура, а **базовый
профиль + расширения по типу проекта**. База должна быть небольшой, а рост
сложности происходить добавлением проверяемых слоев, а не раздуванием
`AGENTS.md`.

## 2. Как проводилось исследование

### 2.1. Источники

Использовались четыре группы источников:

- официальные документы OpenAI, Anthropic, GitHub, Agent Skills и основных
  агентских инструментов;
- публичные описания production-like agent-first практик;
- исследовательские работы 2025–2026 годов;
- выборка собственных репозиториев владельца из `projects-overview.md`.

При конфликте предпочтение отдано официальному текущему контракту инструмента.
Практики, встречающиеся только в community-материалах, помечены как
экспериментальные и не принимаются автоматически в базовый профиль.

### 2.2. Изученная выборка собственных проектов

Сравнены семь agent-first проектов и два human-first контрольных:

- agent-first: `research-workspace`, `size-report`, `biblio-mcp`,
  `browser-agent`, `browser-agent-mcp`, `gemini-mcp`, `freebuff-watchdog`;
- human-first: `figma-safe-resets`, `alfa-ui-kit`.

Выборка покрывает research workspace, CLI/CI, MCP, browser automation,
gateway, cross-repository contract и ранний unattended automation. Это не
оценка качества репозиториев, а сравнение организационных паттернов.

### 2.3. Ограничения

- GitHub Projects собственных репозиториев не были полностью прочитаны через
  API: текущая локальная auth-сессия не имеет `read:project`. Сравнение их
  файловой структуры не заменяет аудит фактических Project boards.
- Поведение инструкций зависит от конкретного harness и его версии; имя файла
  само по себе не гарантирует загрузку.
- Доказательная база по agent-first процессам моложе и слабее, чем классика
  software engineering. Часть выводов — инженерная синтезация, а не
  универсально подтвержденный закон.

## 3. Главный архитектурный принцип: разделить источники истины

Самая опасная ошибка в вашем контексте — не отсутствие файла, а наличие двух
файлов или систем, которые претендуют на одну и ту же истину. При отсутствии
ручного просмотра диффов и PR это приводит к тихому расхождению, которое
некому заметить.

### 3.1. Предлагаемая карта владения данными

| Сведения | Канонический источник | Что может быть в репозитории | Что не следует дублировать |
|---|---|---|---|
| Цель продукта/репозитория | README проекта или `docs/product.md` | краткая ссылка в `AGENTS.md` | не копировать в Project как длинный текст |
| Единица работы, проблема, acceptance criteria | GitHub Issue | ссылка на Issue в плане/state | не заводить параллельный `TODO.md` |
| Иерархия большой задачи | Parent Issue + sub-issues | ссылка на parent в документации | не клонировать задачи в Project |
| Очередь, Status, Priority, Iteration, Estimate | GitHub Project / issue fields | только ссылки, реестр схемы и automation-код | не хранить второй backlog |
| Архитектура и инварианты | `docs/architecture.md` или `ARCHITECTURE.md` | ссылка из `AGENTS.md` | не зашивать все правила в `AGENTS.md` |
| Принятое техническое решение | `docs/decisions/` или `docs/decisions.md` | ссылка на связанные Issue/PR | не переписывать решение в каждом плане |
| Длинная текущая работа | `docs/plans/active/<id>-*.md` | ссылка из Issue и state | не превращать план в список всех задач |
| Текущее состояние и handoff | `docs/state.md` | компактный пакет указателей, блокеров и next action | не считать его свежей истиной о GitHub |
| Повторяемая процедура | `.agents/skills/`, `scripts/`, CI | описание запуска | не копировать процедуру в README/AGENTS/Issue |
| Фактическое выполнение | код, конфигурация, runtime | report/evidence links | не считать заявление агента доказательством |
| Проверка результата | CI Checks, Actions artifacts, smoke/evals | ссылка/идентификатор проверки | не писать «тесты пройдены» без check ID |
| Состояние внешнего сервиса | внешний сервис/API | handle + команда reverify | не полагаться на вчерашнюю заметку |

Эта таблица — рабочая гипотеза для будущего регламента. Особенно важно
разделить **Issue как смысл задачи** и **Project как управляемое состояние ее
очереди**.

### 3.2. Что означает «один источник истины» для GitHub Projects

Официальная документация GitHub прямо рекомендует single source of truth и
указывает, что данные Issue/PR, такие как labels, assignees и milestones,
автоматически отражаются в Project. Поэтому не следует одновременно хранить,
например, два независимых `Priority`, два `Target date` или два описания
acceptance criteria.

Рабочая модель для ваших проектов:

- **Issue** содержит контекст, границы, acceptance criteria, ссылки на
  связанные материалы и условия остановки.
- **Project** содержит очередь, операционный status и project-owned metadata.
- **PR** является execution artifact Issue, а не второй задачей по умолчанию.
- **Checks/Actions** являются доказательством автоматической проверки.
- **Docs** содержат устойчивое знание, которое переживает конкретную задачу.

Если одна задача входит в несколько Projects, проектные поля могут законно
отличаться. Поэтому shared metadata вроде Priority и Effort, если доступна
организационная модель GitHub Issue fields, лучше хранить на Issue. Локальные
поля вроде Iteration конкретного Project могут оставаться project-scoped.

## 4. Влияние GitHub Projects на планы и TODO

### 4.1. Рекомендация

**Не делать `TODO.md` каноническим списком задач.** Для новых проектов его
лучше не создавать вообще. Все долговечные work items должны быть GitHub
Issues, автоматически или идемпотентно включаемые в Project.

`TODO.md` допустим только в трех случаях:

1. это короткий локальный список не-работы, например «проверить варианты
   настройки на этой машине», который не должен жить в GitHub;
2. это сгенерированный read-only отчет, построенный из Issues/Project и
   помеченный как generated;
3. это миграционный остаток, который должен быть разобран и удален.

Не следует использовать Markdown task list (`- [ ]`) как вторую систему
управления, если та же задача существует в Project.

### 4.2. Когда нужен план в репозитории

План нужен, когда работа:

- занимает более одного agent session;
- имеет несколько зависимых этапов;
- затрагивает несколько репозиториев или внешних систем;
- требует архитектурного решения до реализации;
- может быть безопасно продолжена другим агентом;
- имеет дорогой или рискованный verification loop.

Для простой Issue достаточно acceptance criteria и стандартного протокола
агента. Для сложной Issue создается, например:

```text
docs/plans/active/123-rework-transport.md
```

В плане должны быть:

- ссылка на Issue и parent, если он есть;
- цель и non-goals;
- декомпозиция на checkpoints;
- критерии приемки по каждому checkpoint;
- команды проверки;
- зависимости и рискованные действия;
- `Progress`;
- `Surprises & Discoveries`;
- `Decision Log`;
- `Next action`;
- условия завершения и способ архивирования.

После завершения план перемещается в `docs/plans/completed/` либо остается
историческим артефактом с явным статусом. Перемещение не должно создавать
новую Issue или новую карточку.

### 4.3. Что делать с `state.md`

`state.md` не заменяет GitHub Project. Он нужен, потому что агенту требуется
быстрый local-first handoff после нового контекста или сбоя. Его рекомендуемый
размер — небольшой, а содержимое — не backlog, а пакет навигации:

```yaml
status: active
current_goal: "..."
active_issue: "https://github.com/.../issues/123"
active_plan: "docs/plans/active/123-...md"
next_action: "..."
blockers:
  - "..."
last_verified_at: "2026-10-17T...Z"
reverify:
  - "gh issue view 123 --json state,updatedAt"
  - "gh project item-list ..."
evidence:
  - "https://github.com/.../actions/runs/..."
```

Принцип: `state.md` говорит **куда смотреть и что перепроверить**, а не
утверждает, что внешний статус по-прежнему таков. В частности, старое
«ожидает review» нельзя принимать без обращения к GitHub: Issue/PR мог быть
закрыт, слит или заменен.

## 5. Предлагаемый базовый профиль каталогов

Ниже не starter kit для немедленного копирования, а кандидат на матрицу.
Конкретные каталоги добавляются только при наличии соответствующего профиля.

```text
project/
├── AGENTS.md                         # короткая карта и рабочий контракт агента
├── README.md                         # вход для пользователя и агента
├── .gitignore
├── .gitattributes                    # если нужны переносимость/нормализация
├── <manifest>                        # package.json, pyproject.toml, Package.swift...
├── <lockfile>                        # lockfile соответствующего менеджера
├── src/ или app/                     # production code, профиль зависит от стека
├── tests/ или test/                  # проверяемое поведение
├── scripts/                          # deterministic setup/verify/migration helpers
├── docs/
│   ├── README.md                     # карта документации
│   ├── architecture.md              # как устроено и какие границы обязательны
│   ├── state.md                      # компактный handoff-пакет
│   ├── decisions.md или decisions/   # принятые решения и rationale
│   ├── product.md или requirements/  # durable product intent, если применимо
│   ├── plans/
│   │   ├── active/
│   │   └── completed/
│   ├── blockers/                     # только активные внешние/системные блокеры
│   ├── worklog/                      # только если нужен resumable operational log
│   ├── generated/                    # генерируемые docs, если они проверяются
│   └── references/                   # объемные справочные материалы
├── .github/
│   ├── workflows/
│   │   ├── ci.yml                    # канонический обязательный gate
│   │   └── ...                       # release/deploy/maintenance по профилю
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug.yml
│   │   ├── feature.yml
│   │   └── task.yml
│   ├── copilot-instructions.md       # optional thin adapter
│   ├── instructions/                 # optional path-scoped adapters
│   └── dependabot.yml                # optional dependency maintenance
├── .agents/
│   └── skills/                       # optional portable task workflows
└── <platform-specific dirs>          # .devcontainer, infra, deploy, etc.
```

### 5.1. Минимальный профиль для небольшого проекта

```text
AGENTS.md
README.md
<manifest> + <lockfile>
src/ или app/
tests/ или test/
scripts/verify
docs/architecture.md
docs/state.md
.github/workflows/ci.yml
```

`docs/decisions.md`, plans, blockers, issue forms и skills появляются при
первой реальной потребности. Нельзя добавлять пустые «обязательные» файлы
только ради симметрии.

### 5.2. Профили расширения

| Профиль | Добавить | Специфический контроль |
|---|---|---|
| CLI/library | fixtures, package acceptance, consumer smoke | опубликованный artifact и install test |
| Web/UI | e2e, screenshots/visual, local server script | browser-driven smoke |
| Service/API | contract schema, integration tests, health probe | compatibility matrix |
| MCP/tool | client examples, stdio/transport tests, redaction rules | real tool call smoke |
| Research/data | evidence/, source manifest, reproducible run metadata | provenance and rerun checks |
| Desktop/macOS | platform scripts, packaging, runtime smoke | supported OS matrix |
| Multi-repo | `docs/contracts/`, companion links | cross-repo contract test |
| Unattended agent | watchdog, retry policy, run ledger | stop-loss and recovery tests |
| Agent as primary user | examples framed as tool calls, machine-readable output | schema/CLI contract |
| Human as primary user | user docs, screenshots, accessibility | user-flow/e2e evidence |

## 6. Ключевые файлы: рекомендуемая матрица обязательности

### 6.1. Корневой `AGENTS.md`

**Статус:** обязательный для всех проектов, но короткий.

**Роль:** карта, рабочий контракт и навигация агента. Он не должен быть
полной документацией проекта.

**В корне должны быть:**

- назначение проекта в двух-трех предложениях;
- ключевой пользователь: человек, агент или смешанный режим;
- карта важных каталогов;
- точные команды bootstrap, dev, verify, test, build и release, если они
  существуют;
- порядок проверки и единственная каноническая команда общего gate;
- архитектурные инварианты и границы, которые нельзя нарушать;
- правила секретов, generated/vendor/private каталогов;
- модель работы с Issue/Project, включая ссылку на source-of-truth policy;
- ссылки на `docs/architecture.md`, `docs/state.md`, decisions и активный
  план, если он есть;
- definition of done и обязательный финальный отчет агента;
- условия, когда агент должен остановиться и запросить владельца.

**В корне не должны жить:**

- полный backlog;
- длинный исторический worklog;
- копия README, architecture или style guide;
- общие советы, которые агент и так может вывести из проекта;
- обещания, которые не проверяются CI или runtime;
- vendor-specific frontmatter, если это должен быть переносимый файл;
- токены, credentials и реальные payload.

Рабочая целевая длина для кросс-платформенного baseline — примерно 80–150
строк. Это не hard limit, а сигнал к декомпозиции. Важно проверять не число
строк, а то, загружается ли файл целиком и остается ли он исполнимым в
нескольких harness.

### 6.2. Нативные файлы инструкций

**Статус:** условные адаптеры, не второй источник правил.

Основным portable layer следует считать plain Markdown `AGENTS.md`. Нативные
файлы появляются только когда они дают возможность, которой нельзя выразить
через общий файл:

| Harness | Нативный слой | Рекомендация |
|---|---|---|
| Codex/OpenCode/многие другие | `AGENTS.md` | основной общий слой |
| Claude Code | `CLAUDE.md`, `.claude/rules/` | thin adapter или path-specific rules |
| GitHub Copilot | `.github/copilot-instructions.md`, `*.instructions.md` | только Copilot additions |
| Cursor | `.cursor/rules/*.mdc` | только globs/activation, не копия всего baseline |
| Gemini CLI | `GEMINI.md` и imports | использовать настройку на `AGENTS.md`, если возможно |
| GitHub custom agents | `.github/agents/*.md` | специализированные роли, не общий policy |
| Agent Skills | `.agents/skills/*/SKILL.md` | повторяемые процедуры on demand |

Дублирование допустимо как **тонкий адаптер**: один-два абзаца, ссылка на
канонический файл, плюс нативная настройка. Полная копия `AGENTS.md` в
`CLAUDE.md`, `.cursor/rules` и `copilot-instructions.md` создает drift.

Симлинки могут уменьшить дублирование, но не являются надежным универсальным
механизмом: они ведут себя по-разному в Windows, Git checkout и отдельных
инструментах. Для общей матрицы предпочтительнее один реальный файл,
поддерживаемые imports или генерация адаптеров с проверкой равенства.

### 6.3. `README.md`

**Статус:** обязательный baseline, даже если основной пользователь — агент.

README должен отвечать на четыре вопроса:

1. что это и для кого;
2. как запустить минимальный сценарий;
3. где находится документация;
4. как выглядит успешный результат.

Для agent-only внутренних инструментов допустим очень короткий README, но не
нулевой: он дает внешнему агенту и будущему владельцу stable entry point.

### 6.4. `docs/architecture.md`

**Статус:** обязательный после появления нетривиальной архитектуры.

Документ должен описывать не каждый модуль, а границы, направленность
зависимостей, ключевые контракты, внешние системы, способы запуска и
инварианты. Если архитектура сложная, разбивать по доменам и поддерживать
`docs/architecture/index.md`.

### 6.5. `docs/state.md`

**Статус:** рекомендуемый baseline для долгоживущих проектов, обязательный для
unattended/multi-session профиля.

Держать коротким, актуализировать при checkpoint и обязательно указывать
`updated_at`, `last_verified_at`, external handles и reverify commands.

### 6.6. `docs/decisions.md` или `docs/decisions/`

**Статус:** условный, но предпочтительный при архитектурных развилках.

Для малых проектов достаточно одного файла с записями:

```markdown
## D-001: Название решения

- Status: accepted | superseded | rejected
- Date: 2026-10-17
- Context: ...
- Decision: ...
- Consequences: ...
- Evidence: ...
- Related issues: #123
```

Когда решений много или их нужно искать отдельно, переходить к
`docs/decisions/D-001-*.md` и index.

### 6.7. `docs/plans/`

**Статус:** условный профиль для multi-session работы.

Разделять `active/` и `completed/`, не использовать для backlog и не создавать
по плану на каждую маленькую Issue. План должен быть самодостаточным для
замены агента и иметь критерии checkpoint.

### 6.8. `docs/blockers/` и `docs/worklog/`

**Статус:** условные.

- `blockers/` — только активные препятствия, которые требуют внешнего
  действия, нестабильной среды, недоступного сервиса или решения владельца.
- `worklog/` — только когда narrative нельзя заменить коротким `state.md` и
  GitHub timeline. Старые записи архивировать или агрегировать.

Обычная задача не должна порождать одновременно Issue, Project item, plan,
worklog-файл, blocker и TODO. Каждый артефакт должен иметь отдельную роль.

### 6.9. `scripts/verify` и CI

**Статус:** обязательный для любого проекта с кодом.

Должна существовать одна каноническая команда полного локального или CI gate:

```text
./scripts/verify
# или make verify / pnpm verify / task verify / uv run verify
```

Если команды платформы разные, `AGENTS.md` перечисляет их, но CI должен
вызывать тот же профиль, а не собственную непохожую последовательность.

Лестница проверки обычно такая:

1. syntax/format/static checks;
2. unit/contract tests;
3. build/package acceptance;
4. integration/e2e/smoke;
5. live/external checks — только если нужны и с явной маркировкой;
6. security/secrets/dependency checks.

При недоступности внешней среды не маскировать пропуск как pass. Записывать
`blocked`/`not run`, причину и следующий способ проверки.

### 6.10. GitHub issue forms и шаблоны

**Статус:** рекомендуемый baseline, особенно в вашем режиме без просмотра
кода.

Шаблон Issue должен заставлять агента или пользователя заполнить:

- intent/problem;
- scope и non-goals;
- acceptance criteria;
- required evidence/checks;
- affected surface;
- dependencies;
- risk/approval requirement.

PR template должен быть не просьбой к человеку «посмотрите diff», а
машинно-читаемым completion report: ссылка на Issue, измененный surface,
checks, evidence, known gaps, blockers и следующий шаг.

## 7. Как должен выглядеть корневой `AGENTS.md`

Ниже кандидат на структуру, которую позже можно превратить в шаблон. Названия
заголовков не являются частью межинструментального протокола; важна роль
содержания.

```markdown
# Project instructions

## 1. Scope and reading order
- This file is the short operating contract.
- Read `docs/state.md` first for current handoff.
- Read `docs/architecture.md` for system boundaries.
- Read the linked GitHub Issue and active plan before changing files.

## 2. Project identity
- Purpose: ...
- Primary user: human | agent | mixed
- Runtime/platform: ...

## 3. Repository map
- `src/`: ...
- `tests/`: ...
- `scripts/`: ...
- `docs/`: ...

## 4. Commands
- Bootstrap: `...`
- Dev: `...`
- Fast verify: `...`
- Full verify: `...`
- Build/package: `...`

## 5. Work protocol
- Work from the assigned GitHub Issue.
- Do not create a duplicate TODO item for an existing Issue.
- Use an active plan for multi-session work.
- Update state only with verified facts and re-verification handles.

## 6. Boundaries
- Never commit secrets or real credentials.
- Do not modify generated/vendor files manually.
- Do not change public contracts without tests and docs.
- Stop before external/destructive/high-cost actions listed here: ...

## 7. Definition of done
- Acceptance criteria are satisfied.
- The canonical verification command passed, or every gap is explicit.
- Relevant docs/state/decision files are updated.
- The final report contains checks, evidence, remaining work and next action.

## 8. Escalation
- Ask the owner when ...
- Treat these as blockers, not as silent assumptions: ...

## 9. Canonical references
- Project management policy: `docs/project-management.md`
- State: `docs/state.md`
- Architecture: `docs/architecture.md`
- Decisions: `docs/decisions.md`
```

В реальном файле не следует оставлять фиктивные команды или универсальные
советы. Неверная команда хуже отсутствующей: агент потратит попытки на
восстановление несуществующего workflow.

## 8. Что подтверждает собственная выборка

### 8.1. Сильные паттерны

- `research-workspace` показывает ценность state machine, checkpoint,
  resumability, blockers, artifact provenance и разделения agent roles.
- `size-report` показывает силу одного канонического verify-gate, fixtures,
  baselines и проверяемых docs/commands.
- `biblio-mcp` наиболее близок к полезной компактной модели: `STATE.md`,
  decisions, offline suite, package/artifact acceptance и consumer checks.
- `browser-agent` и `browser-agent-mcp` показывают необходимость contract
  ownership и cross-repository contract tests.
- `gemini-mcp` показывает пользу FR-ID требований, offline-first CI и
  отдельного blocker lifecycle.
- `figma-safe-resets` показывает executable documentation: docs ↔ code,
  manifest ↔ runtime, golden output и mutation checks.

### 8.2. Контрастные слабые паттерны

- `freebuff-watchdog` имеет полезные dry-run и mock E2E, но отсутствие
  `AGENTS.md`, CI, canonical state и docs index затрудняет продолжение работы
  новым агентом.
- В нескольких больших проектах текущее состояние распределено между
  blockers, plans и worklog. Это работает, пока процесс дисциплинирован, но
  увеличивает стоимость ориентации.
- Большие `AGENTS.md` с историей и локальными нюансами постепенно становятся
  привлекательной, но плохо проверяемой свалкой знаний.

Главный локальный вывод совпадает с внешним: полезнее не максимальное число
файлов, а ясные роли, один source of truth и маленькая стартовая поверхность.

## 9. Результаты внешнего исследования

### 9.1. `AGENTS.md` — общий знаменатель, но не стандарт семантики

`AGENTS.md` — plain Markdown без обязательной схемы. Он стал наиболее
переносимым именем, потому что его понимают Codex, OpenCode, Cursor, Gemini,
Copilot и другие инструменты. Но у них различаются discovery и precedence:

- одни объединяют цепочку parent → child;
- другие выбирают ближайший или первый файл;
- одни загружают вложенные правила on demand;
- другие используют path globs или manual activation;
- наличие `CLAUDE.md` может изменить, будет ли автоматически прочитан
  `AGENTS.md`.

Следствие: в матрице нужно описывать не только filename, но и **activation
contract** конкретного harness. Для переносимости общий файл должен оставаться
plain Markdown без vendor-specific frontmatter.

### 9.2. Короткая карта лучше монолитной инструкции

OpenAI в описании harness engineering сообщает, что их monolithic `AGENTS.md`
не сработал из-за переполнения контекста, устаревания и невозможности
механической проверки. В итоге корневой файл стал короткой картой примерно на
100 строк, а knowledge base — структурированным `docs/` с architecture,
design docs, product specs, execution plans, quality/reliability/security
документами и generated references.

Это особенно релевантно вашему режиму: вы не будете вручную искать, какой
абзац устарел. Поэтому документы должны иметь scope, владельца, дату
проверки/статус и машинные ссылки.

### 9.3. Не следует автоматически генерировать `AGENTS.md`

Результаты исследований 2026 года пока неоднозначны:

- небольшое эмпирическое исследование с Codex связывает наличие root
  `AGENTS.md` со снижением времени и output token usage на выбранных задачах;
- другая работа на AGENTbench сообщает, что LLM-generated context files могут
  немного ухудшать успех, тогда как human-written дают небольшой выигрыш;
- авторы обеих линий подчеркивают ограничения выборки и необходимость
  проверки на других моделях и задачах.

Практический вывод не «не использовать `AGENTS.md`», а:

- писать его целенаправленно, а не автоматически заполненным обзором;
- включать преимущественно невыводимые из репозитория факты: команды,
  ограничения, внешние контракты, security boundaries, risky actions;
- проверять эффект на representative tasks;
- удалять устаревшие и дублирующие правила.

### 9.4. Progressive disclosure становится базовой архитектурой

Agent Skills specification формализует трехуровневую загрузку:

1. metadata skill загружается постоянно;
2. `SKILL.md` читается при активации;
3. references/scripts/assets загружаются или запускаются только по
   необходимости.

Поэтому файл постоянных инструкций, skill и справочный документ не должны
решать одну задачу одинаковым текстом. В вашей матрице разумно разделить:

- `AGENTS.md` — постоянные правила и карта;
- `.agents/skills/<name>/SKILL.md` — повторяемый workflow;
- `references/` — подробная справка;
- `scripts/` — deterministic operation;
- `docs/` — durable project knowledge.

### 9.5. Долгоживущая работа требует внешнего состояния

Anthropic в работе про long-running agents рекомендует initializer/coding
agent split, progress file, feature list, incremental work, clean state и
проверку базового сценария в начале новой сессии. OpenAI в long-horizon
материале использует spec, milestone plan, implementation runbook,
documentation/status и verification после каждого milestone.

Общий паттерн:

```text
orient → select one bounded unit → implement → verify → repair → checkpoint
       → update state/evidence → stop cleanly
```

Для вашего режима `state.md` и active plan — не декоративная память, а
механизм замены человеческого контроля. При этом каждый checkpoint должен
оставлять не только текстовое обещание, но и evidence handle.

### 9.6. Память должна быть указателем, а не источником текущей истины

Практические обсуждения long-horizon Codex workflows выявляют важную
неочевидную ошибку: даже аккуратный ledger устаревает. Вчерашний «PR waiting
for CI» сегодня может быть merged, closed или заменен. Поэтому долговечный
state packet должен хранить:

- stable handle: Issue/PR number, commit SHA, file path, API query;
- краткое утверждение;
- точную команду или query для повторной проверки;
- дату последней проверки;
- доверенность/evidence, если это важно.

Следующий агент сначала dereference-ит handles, а не действует на narrative.
Это emerging practice с сильной инженерной логикой, но не стандартизированный
протокол; в матрицу ее стоит включить как обязательный принцип state-файлов.

### 9.7. Worker и evaluator должны быть разделены

Long-running harness examples сходятся на разделении ролей:

- planner задает work graph;
- worker меняет проект;
- evaluator начинает с default-fail и требует evidence;
- harness решает retry, checkpoint и escalation.

В репозитории это означает, что «агент сказал done» не меняет статус на
`Done`. Статус меняется только после проверки критериев и машинно наблюдаемого
события. Для high-risk проектов можно добавить отдельный fresh-context QA/eval
агент.

### 9.8. Enforcement нужно выносить из Markdown

Документация направляет агента, но не гарантирует действие. Claude Code прямо
разделяет advisory `CLAUDE.md` и deterministic hooks. OpenAI описывает
mechanical linters, structural tests и doc-gardening. GitHub Agentic Workflows
используют read-only defaults, sandbox, tool allowlisting и safe outputs.

Для ваших проектов:

- «не коммитить секреты» — secret scanner/CI/permissions;
- «не ломать contract» — schema/contract tests;
- «не менять generated files» — generator check или diff guard;
- «обязательно запускать verify» — CI required check или hook;
- «не трогать production» — environment protection и scoped credentials;
- `AGENTS.md` только объясняет, почему и как пройти gate.

### 9.9. Repository hygiene и doc-gardening становятся отдельным процессом

OpenAI описывает регулярный cleanup, quality grading и agent-generated
refactoring PRs, потому что агенты быстро размножают и хорошие, и плохие
паттерны. GitHub Agentic Workflows предлагает continuous documentation,
quality hygiene, test improvement и status reports.

Для вашей матрицы это означает, что «поддерживать структуру» — отдельный
maintenance workflow, а не надежда на то, что каждый feature agent случайно
обновит все документы. Минимальный будущий набор:

- docs link/path/format checker;
- stale state/plan detector;
- orphan Issue/Project reconciliation;
- generated docs check;
- periodic agent task «find drift and open a fix»;
- quality/health report в GitHub Project или Issue.

### 9.10. Agentic Workflows — полезная, но пока emerging практика

GitHub Agentic Workflows в октябре 2026 находятся в public preview. Они
позволяют описывать intent в Markdown в `.github/workflows/`, компилировать его
в Actions lock file и запускать agent в GitHub Actions. Они добавляют
read-only-by-default, sandbox, integrity filter, safe outputs и threat
detection.

Это перспективно для ваших задач:

- ежедневный Project/status report;
- triage Issues;
- анализ failed CI;
- documentation drift;
- low-risk test/maintenance proposals.

Но их нельзя использовать вместо детерминированного CI/CD. Они остаются
subjective/reasoning layer и не должны автоматически менять production или
мержить PR. В матрице их следует поместить в профиль `agentic-maintenance`,
не в обязательный baseline.

### 9.11. Worktrees и изоляция — не только вопрос удобства

Крупные agent-first workflow используют отдельные worktrees или sandbox для
параллельных задач, чтобы не смешивать состояние и не повреждать общий
workspace. В кросс-платформенной матрице нужно описывать не конкретный
провайдер sandbox, а контракт:

- один task/run имеет изолированный рабочий контекст;
- внешние credentials ограничены scope;
- сетевой доступ минимален и явен;
- state/evidence сохраняются вне эфемерной сессии;
- recovery не требует угадывания, что было в предыдущем процессе.

### 9.12. Agent legibility важнее человеческой «красоты»

Для fully agent-generated проектов критерий качества — не только human
readability. Важны:

- predictable directory boundaries;
- stable commands;
- machine-readable outputs;
- explicit contracts;
- discoverable evidence;
- deterministic failures;
- минимальное число скрытых conventions.

Это не оправдывает плохую документацию. Наоборот: верхнеуровневая
документация должна быть короткой и пригодной для редкого человеческого
управления, а техническая детализация — хорошо индексированной для агента.

## 10. Рекомендуемая схема GitHub Project для пилота

Это предварительная схема, которую нужно будет подтвердить отдельным
исследованием и после этого проверить на реальном Project через API.

### 10.1. Scope

- Для одного самостоятельного продукта — один основной Project.
- Для системы из нескольких репозиториев — один Project на продукт/инициативу,
  а не обязательно один на каждый репозиторий.
- Для portfolio уровня — отдельный Project только если действительно нужна
  cross-repo очередь; не смешивать его с локальной очередью реализации.

В личном GitHub-аккаунте может оказаться удобнее user Project; при росте
числа репозиториев organization Project дает более естественный cross-repo
scope. Это зависит от permissions и не решается только структурой файлов.

### 10.2. Items

- Один work item = один обычный Issue.
- Большая работа = parent Issue + sub-issues.
- Draft issue = короткий inbox до triage, не постоянная задача.
- Один Issue добавляется в Project один раз; views — это проекции, а не копии.
- PR показывается через linked PR fields и timeline, не отдельным TODO.

### 10.3. Поля

Кандидат на компактную схему:

| Поле | Владелец | Роль |
|---|---|---|
| Status | Project, partly automation | `Inbox`, `Ready`, `In progress`, `Blocked`, `Verify`, `Done` |
| Priority | Issue field, иначе Project | Urgent/High/Medium/Low |
| Type | Issue type/label | feature/bug/task/research/maintenance |
| Estimate | Issue field или Project number | грубая относительная оценка |
| Iteration | Project | только если нужен календарный ритм |
| Target date | один date field | только если дата реально управляется |
| Risk | Project или Issue field | low/medium/high/approval-needed |
| Agent state | Project | `needs-input`, `waiting-external`, `ready-to-verify` |

Не добавлять поле, если его нельзя использовать в view, automation или
отчетности. Не кодировать status/priority в свободном тексте Issue.

Отдельно нужно решить, использовать ли `Agent state` или достаточно
`Status + labels`. Дублировать два поля с одинаковой семантикой нельзя.

### 10.4. Views

Минимальные views:

- `Inbox` — новые и неразобранные Issues;
- `Agent queue` — Ready/In progress с priority и estimate;
- `Blocked / waiting` — только препятствия, требующие внешнего события;
- `Verification` — Verify и recent failed checks;
- `Recently done` — закрытые/слитые items для автоматического отчета;
- `Portfolio` — только если Project cross-repo.

Не создавать отдельные карточки для каждого view. Фильтры должны быть
машино-проверяемыми и не зависеть от narrative body.

### 10.5. Automation

Базовый порядок:

1. новая подходящая Issue автоматически добавляется в Project;
2. Issue gets `Ready` после triage;
3. назначение агенту переводит в `In progress`;
4. открытие PR или запуск обязательной проверки переводит в `Verify`;
5. failed checks переводят в `Blocked`/`In progress` с evidence link;
6. закрытая Issue/merged PR переводит в `Done`;
7. completed items архивируются по retention policy.

Built-in automation покрывает не все переходы. Для сложных переходов нужен
идемпотентный GitHub Action/GraphQL reconciler. Webhooks следует считать
сигналом перечитать состояние, а не единственным audit log.

## 11. Что сделать обязательным, условным и не рекомендуемым

### Обязательное в матрице

- короткий root `AGENTS.md`;
- README с purpose и quickstart;
- явная карта source of truth;
- один verify gate и CI;
- `docs/state.md` для долгоживущего проекта;
- Issue-driven work и связь с Project;
- acceptance criteria и evidence;
- отсутствие секретов в repo;
- machine-checkable boundaries для критических правил;
- policy «stored state is pointer; reverify before acting».

### Условное по профилю

- nested `AGENTS.md`;
- native instruction adapters;
- `docs/decisions/`;
- active/completed plans;
- worklog и blockers;
- `.agents/skills/`;
- e2e, browser, visual, contract, package and live checks;
- sandbox/worktrees;
- Agentic Workflows;
- custom agents и MCP configuration;
- multi-repo portfolio Project.

### Не рекомендуемое как базовая практика

- большой монолитный `AGENTS.md`;
- `TODO.md` как параллельный backlog;
- копирование одних правил в `AGENTS.md`, `CLAUDE.md`, Cursor и Copilot;
- PR как отдельная задача без самостоятельной управленческой роли;
- план на каждую маленькую Issue;
- narrative `state.md` без handles и reverify commands;
- статус `Done` по заявлению агента без checks;
- reliance on symlinks as the only cross-platform adapter;
- агентское CI вместо детерминированного CI;
- массовое внедрение всех optional files в маленькие репозитории.

## 12. Итоговая рекомендация для вашего контекста

На первом этапе следует принять не файловый шаблон, а **контракт управления
проектом**:

```text
GitHub Issue / sub-issues
        ↓
GitHub Project = queue + operational metadata + views
        ↓
agent run / plan / implementation
        ↓
CI + evaluator + runtime evidence
        ↓
state packet + docs/decisions + Project reconciliation
```

Репозиторий хранит durable knowledge, execution plans и machine-checkable
workflow. GitHub хранит work items и их операционное состояние. Они связаны
ссылками и автоматизацией, но не дублируют друг друга.

Для владельца, который почти не смотрит технические детали, это дает три
уровня обзора:

1. **GitHub Project:** что сейчас происходит, где блокеры и что следующее;
2. **верхнеуровневые docs:** почему проект устроен так и куда он движется;
3. **автоматические evidence:** почему система считает работу завершенной.

## 13. Следующий шаг

Следующим отдельным исследованием стоит разработать и согласовать:

1. детальный шаблон корневого `AGENTS.md`;
2. формальную schema/регламент `docs/state.md`;
3. lifecycle Issue → Project → agent run → verification → Done;
4. набор Project fields, statuses, views и automation;
5. профильные варианты для CLI, UI, MCP, service, research и unattended
   automation;
6. механизм автоматической проверки самой матрицы.

До этого не следует массово перестраивать все 37 репозиториев. Лучше выбрать
2–3 пилота из разных strata (`size-report`, `biblio-mcp`, `freebuff-watchdog`
или `research-workspace`), прогнать новую схему на реальной работе и измерить:

- сколько времени агент тратит на orientation;
- сколько раз возникают дублирующие задачи;
- сколько state claims оказываются stale;
- какие docs реально читаются;
- сколько failures ловятся автоматикой до человеческого вмешательства.

## 14. Источники

### Официальные и первичные источники

1. [OpenAI: Harness engineering](https://openai.com/index/harness-engineering/) —
   agent-first repository, short `AGENTS.md`, structured docs, executable
   plans, mechanical enforcement и doc-gardening.
2. [OpenAI Developers: Run long horizon tasks with Codex](https://developers.openai.com/blog/run-long-horizon-tasks-with-codex) —
   spec, plans, implementation runbook, status/audit log и verification per
   milestone.
3. [Anthropic: Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) —
   initializer/coding split, progress file, feature list, clean state,
   incremental work и fresh end-to-end verification.
4. [GitHub Docs: Best practices for Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/best-practices-for-projects) —
   single source of truth, views, fields, automation, templates и
   cross-repository planning.
5. [GitHub Docs: Managing issue fields](https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/managing-issue-fields-in-your-organization) —
   distinction between issue fields and project fields, their scope and
   limits.
6. [GitHub Docs: Using the API to manage Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects/automating-your-project/using-the-api-to-manage-projects) —
   ProjectV2 items, GraphQL IDs, idempotent add, field updates and webhook
   contract.
7. [GitHub Docs: Copilot best practices for tasks](https://docs.github.com/en/copilot/tutorials/cloud-agent/get-the-best-results) —
   clear Issue description, acceptance criteria, paths, instructions and
   pre-installation.
8. [GitHub Docs: Copilot on GitHub](https://docs.github.com/en/copilot/concepts/copilot-surfaces/copilot-on-github) —
   agent sessions, repository scope, limitations and customizations.
9. [GitHub: Agentic Workflows technical preview](https://github.blog/changelog/2026-02-13-github-agentic-workflows-are-now-in-technical-preview/) —
   Markdown intent, compiled Actions, read-only defaults and safe outputs.
10. [GitHub: Agentic Workflows public preview](https://github.blog/changelog/2026-06-11-github-agentic-workflows-is-now-in-public-preview/) —
    sandboxing, integrity filter, threat detection and public-preview status.
11. [Agent Skills specification](https://agentskills.io/specification) —
    `SKILL.md`, progressive disclosure, references/scripts/assets и validation.
12. [Claude Code memory and instructions](https://code.claude.com/docs/en/memory) —
    loading semantics, concise instructions, `AGENTS.md` compatibility and
    distinction between memory and enforced controls.
13. [OpenAI AGENTS.md documentation](https://learn.chatgpt.com/docs/agent-configuration/agents-md) —
    layered discovery, overrides and context-size behavior.
14. [Cursor Rules](https://cursor.com/docs/rules) — `.mdc`, activation modes,
    path rules и progressive references.
15. [GitHub: Reliable AI workflows and context engineering](https://github.blog/ai-and-ml/github-copilot/how-to-build-reliable-ai-workflows-with-agentic-primitives-and-context-engineering/) —
    Markdown workflows, primitives, context loading and validation gates.

### Исследовательские и community-источники

16. [On the Impact of AGENTS.md Files](https://arxiv.org/html/2601.20404v2) —
    paired Codex study of root `AGENTS.md`; useful evidence, but small sample
    and limited generalization.
17. [Evaluating AGENTS.md](https://arxiv.org/html/2602.11988) —
    contrasting AGENTbench evidence about human-written versus generated
    context files; use as a caution, not a universal verdict.
18. [GitHub analysis of 2,500+ AGENTS.md files](https://github.blog/ai-and-ml/github-copilot/how-to-write-a-great-agents-md-lessons-from-over-2500-repositories/) —
    recurring content areas: commands, tests, structure, style, git workflow
    and boundaries.
19. [SWE-agent](https://github.com/SWE-agent/SWE-agent) — issue-to-patch model,
    trajectories, tests and agent-oriented repository structure.
20. [OpenHands verification stack](https://www.openhands.dev/blog/20260506-the-verification-stack) —
    agent-level verifier, repo-level review and evidence-oriented QA.

### Собственные источники

21. [`projects-overview.md`](../../projects-overview.md) — inventory 37
    repositories and stratification context.
22. [`context.md`](context.md) — initial brief and constraints for
    this methodology project.
23. Representative repositories listed in section 8, inspected through their
    repository metadata and documentation files during this research pass.

## 15. Research rerun metadata

```yaml
workflow: agent-first-structure-and-files
date: 2026-10-17
depth: exhaustive
scope:
  - cross-platform AI coding agent conventions
  - long-running agent state and resumability
  - GitHub Projects source-of-truth design
  - local representative repositories
output: markdown
status: working-recommendations
next: design-and-agree-root-agents-and-project-lifecycle
```
