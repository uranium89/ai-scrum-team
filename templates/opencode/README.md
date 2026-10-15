# AI Scrum Team for OpenCode

## Setup
1. Copy everything in this folder, including the hidden `.opencode/`, into your project root.
2. OpenCode loads `AGENTS.md` and the subagents in `.opencode/agents/` automatically.

## Prompts (in the Build agent)
**1. Kickoff** (once per project):
```
Kick off a new product: [WHAT YOU WANT, in your own words].
Act as the orchestrator defined in AGENTS.md and run the Kickoff.
Invoke each subagent via the Task tool, one at a time.
Ask me the business interview questions and wait for my answers. Then finish the Kickoff and show me the backlog, any decision briefs and the first sprint plan.
```

**2. Run a sprint** (after you reply "approved" to the plan):
```
Run sprint [NN] as defined in AGENTS.md, story by story.
Invoke each subagent via the Task tool, one at a time.
Stop when the sprint is finished or nothing can proceed, then show me the sprint review.
```

**3. Quick change** (small, low-risk):
```
quick: [the change]
```

{{include shared/docs/WORKFLOW.md}}

## OpenCode notes
- This is the template with the strongest permission limits (bash rules: the last matching pattern wins):
  - product-owner, architect and reviewer may run only read-only commands (`rg`, `ls`, `cat`, `git status/diff/log`, `command -v`).
  - devsecops may run the scanners and read-only commands freely; any other command asks you first.
  - coder, backend-dev, frontend-dev and tester may run any command except `git push`.
- The `edit` permission cannot be scoped to a folder, so every agent can still write anywhere; write zones are instructions.
- Models: `anthropic/claude-opus-5-5` for the strong roles, `anthropic/claude-sonnet-5-5` for the developers and tester. Use `provider/model-id` to switch providers; giving the reviewer a different vendor's model reduces shared blind spots.
- If a project has both `CLAUDE.md` and `AGENTS.md`, OpenCode reads only `AGENTS.md`.
