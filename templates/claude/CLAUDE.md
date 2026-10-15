# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

{{include shared/RULES.md}}

## Orchestration (Claude Code)
- The roles are subagents in `.claude/agents/*.md`: `product-owner`, `architect`, `coder`, `backend-dev`, `frontend-dev`, `ui-designer`, `tester`, `devsecops`, `reviewer`.
- The main session is the orchestrator. Invoke the agents with the Agent tool, following the Orchestrator rules above.
- Agent front matter uses the aliases `model: opus` / `model: sonnet`, which resolve to the current model in each family.
