# NOTES

## What this plugin does
`productivity` bundles a pre-PR review workflow plus a standup helper into one
installable Claude Code plugin:
- `/productivity:pre-pr-review` — runs a diff review, lint check, and unit
  tests against the branch, then drafts a PR description from the results.
- `/productivity:standup` — summarizes your recent commits across repos.
- A `pretest-git-add` hook that runs the relevant test file before a
  `git add`, so failing tests are caught before they're staged.

## How to install
From a fresh Claude Code session:
```
/plugin marketplace add https://github.com/thunderTheNakedGun/claude-multi-agent-workflow.git
/plugin install productivity@productivity-marketplace
```
To test locally against this repo's own `course-api/` app without publishing
first, run `claude --plugin-dir .` from the repo root, then
`cd course-api && npm install` once before invoking any command.

## One scoping decision
`diff-reviewer` is scoped to `Read, Grep, Glob` only — no `Bash`, no `Edit`.
Its job is a second opinion on code before a PR opens, and giving it write
access would let a review silently turn into an edit, hiding what it changed
from the person relying on its judgment. `unit-tests`, by contrast, gets
`Bash, Read, Edit` because its job — repair failures it can fix — requires
writing code and re-running the suite to confirm the fix. `lint` runs on
`model: haiku` rather than `sonnet` because "run npm run lint and report
pass/fail" is mechanical pattern-matching over command output, not judgment
work, so the cheaper model is enough.

## One orchestration decision
`pre-pr-review` runs `diff-reviewer`, `lint`, and `unit-tests` in parallel —
they inspect independent concerns (code correctness, style, and test
behavior) and none needs another's output, so running them together cuts
wall-clock time with no cost. `pr-summarizer` runs afterward, sequentially,
because a PR description that claims "tests pass" or "lint is clean" has to
be true — it needs the actual pass/fail results and findings from the other
three before it can honestly describe the state of the branch.
