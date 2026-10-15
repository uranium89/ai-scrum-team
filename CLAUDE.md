# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A set of templates for an AI Scrum team (product-owner, architect, coder or backend-dev + frontend-dev, tester, devsecops, reviewer) working for a non-technical stakeholder, packaged for Claude Code, Codex, Cursor, OpenCode and Antigravity. The workflow rules themselves live in `shared/RULES.md`; this file is about maintaining the templates, not running the workflow.

## Commands

- `python3 scripts/build.py`: regenerate `dist/` from `shared/` + `templates/`.
- `python3 scripts/build.py --check`: exit 1 if `dist/` is stale. Run it after every change.

The script needs only the Python standard library (3.9+).

## Architecture

- `shared/` holds everything common to all tools: `RULES.md`, the nine agent prompts (`shared/agents/*.md`, front matter holds only `description`), README sections (`shared/docs/`), and the project skeleton plus example story (`shared/project/`, copied into every `dist/<tool>/ai-team/`).
- `templates/<tool>/` mirrors the output layout exactly and holds only tool-specific parts: agent front matter or TOML fields, the rules-file wrapper and its "Orchestration" section, and the README (`templates/<tool>/ai-team/README.md`).
- `templates/` files pull in shared content with `{{include PATH}}`, `{{body PATH}}` and `{{description PATH}}` (paths relative to the repo root). See the docstring in `scripts/build.py`.
- `dist/` is generated output. Never edit it by hand.

## Conventions

- All documentation, prompts and templates are written in English.
- Put a change in `shared/` whenever it applies to more than one tool; keep `templates/` to what genuinely differs per tool.
- Tool config formats (front matter fields, model IDs, tool names) change often. Check the tool's official docs before editing them; the Antigravity `tools` list in particular is unverified beyond the four documented names.
- The stakeholder is non-technical. Anything the workflow asks of them (kickoff interview, decision briefs, sprint review) must stay in business terms: cost, time, risk, reversibility.
- A role's write zone, model tier (strong or fast) and duties must agree across `shared/RULES.md`, `shared/docs/WORKFLOW.md`, the agent prompt and every tool's agent wrapper. For OpenCode, bash rules use last-match-wins.
