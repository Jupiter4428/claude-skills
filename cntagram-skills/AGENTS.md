# Agent instructions

## Skills are mandatory

This repo ships its own skills in `.claude/skills/`. They are the required workflow, not suggestions.

Before starting any task:

1. Find the matching skill in the table below. If unsure which fits, read `.claude/skills/ask-matt/SKILL.md` first — it routes to the right one.
2. Load that skill (Claude Code: the Skill tool; other agents: read its `SKILL.md` in full, plus any files it links to) before writing code or docs.
3. Follow its steps. If you skip or deviate from a step, say so and why.

While working, **tune the skill on every miss**: when an error, a failed check, or a user correction traces back to a skill (a step that was wrong, missing, or easy to misread), fix that skill's `SKILL.md` before moving on. Load `writing-for-agents`, make the smallest edit that would have prevented the miss, keep it in its own `chore(skills): ...` commit, and name the edit in your next message. Misses with no skill behind them (a typo, a one-off code bug) leave the skills as they are.

| Situation | Skill |
| --- | --- |
| Building a feature or fixing a bug | `tdd` |
| Something broken, throwing, failing, or slow; a stack trace or error log | `diagnosing-bugs` |
| Reviewing a branch, PR, WIP changes, a plan or a design doc | `code-review` |
| Bug fixed and validated; writing the root-cause record | `post-mortem` |
| Rewriting engineering work for PM / lecturer / leadership | `management-talk` |
| Designing or reshaping a module's interface | `codebase-design` |
| Domain terms, `CONTEXT.md`, or ADRs | `domain-modeling` |
| Researching docs/APIs/a topic | `research` |
| A design question that needs runnable code to answer | `prototype` |
| Writing a PR body or a review of someone's PR, or answering PR review comments | `pr` |
| Steps only a human can do (secrets, dashboards, deploy cutover) | `wizard` |
| In-progress merge or rebase conflict | `resolving-merge-conflicts` |
| Looping, re-reading the same files, or context running low | `qwenchance` |
| Editing skills, `AGENTS.md`, or `CLAUDE.md` | `writing-for-agents` |
| Unsure which applies | `ask-matt` |

User-typed only (the agent cannot start these; suggest them when they fit): `/grill-with-docs`, `/to-spec`, `/to-tickets`, `/implement`, `/implement-spec`, `/triage`, `/wayfinder`, `/retro`, `/handoff`, `/wait-what`, `/to-questionnaire`, `/improve-codebase-architecture`, `/setup-matt-pocock-skills`.

A repo skill takes precedence over a built-in or global skill with the same purpose (e.g. use `.claude/skills/code-review`, not Claude Code's built-in `/code-review`).

Situation-by-situation guide: `.claude/skills/README.md`.

## Review loop

A code change is done only after **two review rounds**. Once the work is built and its tests pass:

1. **Review.** Load `code-review` with fixed point `develop`. If the work is uncommitted, include the working tree: diff against `git merge-base develop HEAD`, not `HEAD`.
2. **Fix.** Fix every blocker and major finding (behaviour changes go through `tdd`); fix each nit or state why it stays. Done when the tests pass again.
3. **Repeat** steps 1–2 once more as round 2.

A finding that needs a human decision (a product call, a trade-off, a spec ambiguity) goes on a **parked** list; keep going with the rest of the cycle. Ask the parked questions only after round 2 ends.

When reporting back, list per round: findings, what you fixed, and what you left and why; then the parked questions.
