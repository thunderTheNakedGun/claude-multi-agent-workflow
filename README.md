# productivity

A Claude Code plugin bundling a multi-agent productivity workflow — scoped subagents, an orchestration command, a skill, and a hook.

## Components

- `agents/` — `diff-reviewer` (read-only), `lint`, `unit-tests` (write-capable), `pr-summarizer`
- `commands/` — `/productivity:pre-pr-review` (the workflow command) and `/productivity:standup`
- `skills/` — `pr-description`, `commit-messages`
- `hooks/` — `pretest-git-add`, runs related unit tests before `git add`

See `NOTES.md` for what each command does, plus the scoping and orchestration decisions behind them.

## Install

From a Claude Code session:
```
/plugin marketplace add https://github.com/thunderTheNakedGun/claude-multi-agent-workflow.git
/plugin install productivity@productivity-marketplace
```

To test locally against this repo's own `course-api/` app without publishing first, run `claude --plugin-dir .` from the repo root, then `cd course-api && npm install` once before invoking any command.
