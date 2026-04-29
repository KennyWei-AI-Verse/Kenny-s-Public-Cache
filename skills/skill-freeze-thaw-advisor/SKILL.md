---
name: skill-freeze-thaw-advisor
description: Lightweight advisor for managing AI agent skills in a freeze/thaw workflow. Use when the user asks which skills should stay active, which skills can be disabled, whether a task needs a specific skill, or when they want reminders to temporarily enable a skill and disable it again after the task.
version: "1.0.0"
author: "OpenClaw User"
---

# Skill Freeze/Thaw Advisor

Use this skill to keep an agent's skill set lean without losing specialized capabilities.

The core idea: **keep only a small set of baseline skills active, temporarily enable specialized skills when a task needs them, then disable them again after the task is done.**

## When to use

Use this skill when the user asks about:

- which skills are currently worth keeping active
- which skills can be disabled or frozen
- whether a specific task needs a specialized skill
- why too many active skills may slow the agent down
- building a lightweight skill management workflow
- reminding the user to disable a temporary skill after a task

Do not use this skill for ordinary business tasks unless the user explicitly asks about skill management.

## Principles

- Keep the active skill set small.
- Do not turn this skill into a global router for every task.
- Recommend enabling at most one specialized skill unless the task clearly spans multiple domains.
- If the base agent can complete the task safely, do not recommend enabling another skill.
- After a temporary skill is used, remind the user to disable it again.
- Never include secrets, private paths, internal hostnames, tokens, API keys, or personal identifiers in public-facing notes.

## Decision workflow

1. Identify the user's task.
2. Decide whether the task needs specialized procedures or safety checks.
3. If yes, suggest the smallest relevant skill to enable temporarily.
4. Explain the reason in one short sentence.
5. Continue only after the user confirms if enabling/disabling requires configuration changes.
6. When the task finishes, remind the user to disable the temporary skill.

## Suggested response patterns

Before a task:

> This task likely needs `{skill-name}` because it contains the relevant workflow/checklist. Enable it temporarily, and disable it again after the task.

If no extra skill is needed:

> No extra skill is needed for this. I can handle it with the current lightweight setup.

After a task:

> This task is done. You can disable `{skill-name}` again to keep the agent lightweight.

## Common mappings

| Task type | Suggested temporary skill |
|---|---|
| Multi-step browser automation, login checks, UI flows | `browser-automation` |
| Static site, landing page, dashboard, React/Tailwind UI | `frontend-design` or equivalent frontend skill |
| Python coding, review, refactor, tests | `python` |
| GitHub issues, PRs, CI, `gh` workflows | `github` |
| Creating or updating a skill | `skill-creator` |
| Reviewing an external skill before install | `skill-vetter` |
| Host security audit, firewall, SSH, exposed ports | `healthcheck` |
| Device/node pairing and connection failures | `node-connect` |
| Long-running tasks with waits, state, and resumability | `taskflow` |
| Inbox/message triage workflows | `taskflow-inbox-triage` |
| Memory stack setup or repair | `memory-suite-setup` |
| External web search via a specific provider | search-provider-specific skill |

## What to keep active

A small baseline is usually enough:

- collaboration/plan mode skill, if not already encoded in global instructions
- memory/rules maintenance skill, if the agent frequently curates long-term memory
- this skill, if the user actively manages skill freeze/thaw behavior
- any skill that is genuinely used every day during the current work phase

Everything else should usually be enabled only when needed.

## Output style

Be brief and concrete:

- state the recommendation first
- give one reason
- mention whether to disable the skill afterward
- avoid long tables unless the user asks for a full audit

