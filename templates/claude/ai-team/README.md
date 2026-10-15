# AI Scrum Team for Claude Code

## Setup
1. Copy everything in the template folder (`dist/claude/`, the parent of this `ai-team/` folder), including the hidden `.claude/`, into your project root.
2. Claude Code loads `CLAUDE.md` and the subagents in `.claude/agents/` automatically. Run `/agents` to confirm all eight are listed.

## Prompts
**1. Kickoff** (once per project):
```
Kick off a new product: [WHAT YOU WANT, in your own words].
Act as the orchestrator defined in CLAUDE.md and run the Kickoff.
Invoke each agent as a subagent, one at a time.
Ask me the business interview questions and wait for my answers. Then finish the Kickoff and show me the backlog, any decision briefs and the first sprint plan.
```

**2. Run a sprint** (after you reply "approved" to the plan):
```
Run sprint [NN] as defined in CLAUDE.md, story by story.
Invoke each agent as a subagent, one at a time.
Stop when the sprint is finished or nothing can proceed, then show me the sprint review.
```

**3. Quick change** (small, low-risk):
```
quick: [the change]
```

{{include shared/docs/WORKFLOW.md}}

## Claude Code notes
- Models: product-owner, architect, devsecops and reviewer use `model: opus`; coder, backend-dev, frontend-dev and tester use `model: sonnet`. These aliases resolve to the newest model in each family.
- Agents have every tool available. To restrict one, add a `tools:` list to its front matter.
- For a reviewer from a different vendor, run the review step in another tool (for example the Codex or OpenCode template) on the same repo.
