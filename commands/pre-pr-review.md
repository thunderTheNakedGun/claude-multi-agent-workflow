---
description: Carry out checks before a PR is made - linter check and unit tests, then write the PR description
allowed-tools: Read, Grep, Edit(README.md), Bash, Skill
---

 Run the diff-reviewer, lint, and unit-tests subagents all 3 in parallel — diff-reviewer looks over the files changed on the current branch, while lint and unit-tests check the codebase overall. When all 3 finish, use the pr-summarizer agent to draft the PR description.
