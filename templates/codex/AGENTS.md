{{include shared/RULES.md}}

## Orchestration (Codex)
- The roles are custom agents in `.codex/agents/*.toml`: `product-owner`, `architect`, `coder`, `backend-dev`, `frontend-dev`, `ui-designer`, `tester`, `devsecops`, `reviewer`.
- The main thread is the orchestrator. Spawn ONE subagent per step, following the Orchestrator rules above.
- Subagents inherit the parent's sandbox and approval policy; role write zones are enforced only by these instructions.
