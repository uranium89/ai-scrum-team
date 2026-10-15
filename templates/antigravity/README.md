# AI Scrum Team for Google Antigravity

## Setup
1. Copy everything in this folder, including the hidden `.agents/`, into your project root.
2. Antigravity loads `AGENTS.md` and the custom agents in `.agents/agents/` (`subagent: true`, `mainAgent: false`) automatically.

## Prompts (in the main agent)
**1. Kickoff** (once per project):
```
Kick off a new product: [WHAT YOU WANT, in your own words].
Act as the orchestrator defined in AGENTS.md and run the Kickoff.
Use invoke_subagent for each step, one at a time.
Ask me the business interview questions and wait for my answers. Then finish the Kickoff and show me the backlog, any decision briefs and the first sprint plan.
```

**2. Run a sprint** (after you reply "approved" to the plan):
```
Run sprint [NN] as defined in AGENTS.md, story by story.
Use invoke_subagent for each step, one at a time.
Stop when the sprint is finished or nothing can proceed, then show me the sprint review.
```

**3. Quick change** (small, low-risk):
```
quick: [the change]
```

{{include shared/docs/WORKFLOW.md}}

## Antigravity notes (verify on first run)
- `tools` lists the four tool names from the official docs: `view_file`, `grep_search`, `replace_file_content`, `run_command`. The docs name no tool for creating new files, so agents may need `run_command` for that. The docs also warn that an unknown tool name can hang a subagent; if one hangs, check this list first.
- Models: `pro` for product-owner, architect, devsecops and reviewer; `flash` for the developers and tester.
- `commandExecutionPolicy`: product-owner, architect and reviewer use `sandbox` (the default); devsecops, the developers and tester use `auto` so they can install scanners, build and run tests.
- This template doesn't use Workflows (`/workflow`), which Antigravity stops supporting on 2026-10-19 in favor of Agent Skills.
