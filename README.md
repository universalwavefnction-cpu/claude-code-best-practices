# Claude Code — Best Practices

A distilled, practical guide to working effectively with [Claude Code](https://claude.com/claude-code), Anthropic's agentic coding tool for the terminal. Written for people who have installed it and want to get good fast — not a feature dump, but the habits and patterns that actually move work forward.

> Практическое руководство по эффективной работе с Claude Code. Краткий старт на русском — см. раздел [«Быстрый старт»](#быстрый-старт-quick-start-ru) ниже.

---

## What this is

Claude Code is a coding agent that lives in your terminal. It reads your files, runs commands, edits code, and drives git — with you in the loop. Used well, it compresses hours of work into minutes. Used poorly, it spins on the wrong problem and burns tokens.

This repo is the difference between the two: the working habits, the configuration that pays off, and the failure modes worth knowing before you hit them.

## Contents

| # | Guide | What you'll learn |
|---|-------|-------------------|
| 1 | [Getting Started](docs/01-getting-started.md) | Install, first session, the core loop |
| 2 | [CLAUDE.md — Project Memory](docs/02-claude-md.md) | The single highest-leverage file in your repo |
| 3 | [Effective Prompting](docs/03-effective-prompting.md) | How to ask so you get the right thing |
| 4 | [Context Management](docs/04-context-management.md) | Keeping the agent sharp on long tasks |
| 5 | [Permissions & Safety](docs/05-permissions-safety.md) | Control what the agent can do |
| 6 | [Skills & Slash Commands](docs/06-skills-and-commands.md) | Reusable capabilities and shortcuts |
| 7 | [MCP Servers](docs/07-mcp-servers.md) | Connecting external tools and data |
| 8 | [Subagents & Workflows](docs/08-subagents-workflows.md) | Parallelism and multi-step orchestration |
| 9 | [Git Workflow](docs/09-git-workflow.md) | Commits, branches, and PRs done right |
| 10 | [Troubleshooting](docs/10-troubleshooting.md) | Common failure modes and fixes |

Plus ready-to-copy [`examples/`](examples/): a starter `CLAUDE.md`, a `settings.json`, and a custom slash command.

## The five habits that matter most

If you read nothing else, internalize these:

1. **Write a `CLAUDE.md`.** It's the project's memory. Every session starts by reading it. Put your build commands, conventions, and "don't touch X" rules there. See [guide 2](docs/02-claude-md.md).
2. **Give context, then a clear ask.** "Fix the bug" is weak. "The login form on `auth/login.tsx` rejects valid emails — find why and fix it" is strong. See [guide 3](docs/03-effective-prompting.md).
3. **Let it work, then verify.** The agent is good. Trust it to act on reversible changes, but read the diff and run the tests. Don't micromanage; do confirm.
4. **Keep context clean.** One task per session where you can. Use `/clear` between unrelated jobs. A focused context is a sharp agent. See [guide 4](docs/04-context-management.md).
5. **Stop before destructive actions.** Deletes, force-pushes, prod deploys, secret rotation — these need a human's eyes. Configure permissions so they always ask. See [guide 5](docs/05-permissions-safety.md).

---

## Быстрый старт (Quick Start, RU)

Claude Code — это агент-программист, который работает прямо в терминале. Он читает файлы, запускает команды, редактирует код и управляет git — под вашим контролем.

**Установка:**

```bash
npm install -g @anthropic-ai/claude-code
cd ваш-проект
claude
```

**Первая сессия — три правила:**

1. **Создайте файл `CLAUDE.md`** в корне проекта. Запишите туда команды сборки/тестов, соглашения по стилю кода и то, что трогать нельзя. Claude читает его в начале каждой сессии. → [Подробнее](docs/02-claude-md.md)
2. **Давайте контекст, потом задачу.** Не «исправь баг», а «форма входа в `auth/login.tsx` не принимает валидные email — найди причину и исправь». → [Подробнее](docs/03-effective-prompting.md)
3. **Проверяйте результат.** Агент хорош, но смотрите diff и запускайте тесты. Перед удалением файлов, force-push и деплоем — пусть всегда спрашивает разрешение. → [Подробнее](docs/05-permissions-safety.md)

**Полезные команды в сессии:**

| Команда | Что делает |
|---------|-----------|
| `/clear` | Очистить контекст перед новой задачей |
| `/init` | Сгенерировать стартовый `CLAUDE.md` |
| `/help` | Список команд |
| `Esc` | Прервать текущее действие агента |
| `! команда` | Выполнить shell-команду напрямую |

Дальше читайте руководства в папке [`docs/`](docs/) по порядку.

---

## License

[MIT](LICENSE) — use freely, including commercially. No attribution required.

*Not an official Anthropic project. "Claude" and "Claude Code" are products of Anthropic.*
