{{include shared/RULES.md}}

## Orchestration (Antigravity)
- The roles are custom agents in `.agents/agents/*.md` with `subagent: true`: `product-owner`, `architect`, `coder`, `backend-dev`, `frontend-dev`, `tester`, `devsecops`, `reviewer`.
- The primary agent is the orchestrator. It spawns them with `invoke_subagent`, following the Orchestrator rules above.
