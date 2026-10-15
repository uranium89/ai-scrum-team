{{include shared/RULES.md}}

## Orchestration (OpenCode)
- The roles are subagents in `.opencode/agents/*.md`: `product-owner`, `architect`, `coder`, `backend-dev`, `frontend-dev`, `ui-designer`, `tester`, `devsecops`, `reviewer`.
- The primary agent (Build) is the orchestrator. It invokes them via the Task tool, following the Orchestrator rules above; the stakeholder can also @mention them.
