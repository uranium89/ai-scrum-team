# AI Scrum Team for OpenAI Codex

## Setup
1. Copy everything in the template folder (`dist/codex/`, the parent of this `ai-team/` folder), including the hidden `.codex/`, into your project root.
2. Codex loads `AGENTS.md` and the custom agents in `.codex/agents/*.toml` automatically.

## Prompts
**1. Kickoff** (once per project):
```
Kick off a new product: [WHAT YOU WANT, in your own words].
Act as the orchestrator defined in AGENTS.md and run the Kickoff.
Spawn subagent for each step, one at a time, and wait for it to finish.
Ask me the business interview questions and wait for my answers. Then finish the Kickoff and show me the backlog, any decision briefs and the first sprint plan.
```

**2. Run a sprint** (after you reply "approved" to the plan):
```
Run sprint [NN] as defined in AGENTS.md, story by story.
Spawn subagent for each step, one at a time, and wait for it to finish.
Stop when the sprint is finished or nothing can proceed, then show me the sprint review.
```

**3. Quick change** (small, low-risk):
```
quick: [the change]
```

Use `/subagents` to switch between agent threads.

{{include shared/docs/WORKFLOW.md}}

## Codex notes
- Codex only spawns subagents when asked explicitly, so the prompts say "Spawn subagent".
- Subagents share the main session's sandbox and approval policy; an agent file cannot restrict its own permissions. Set permissions for the main session with `/permissions`.
- Models: product-owner, architect, ui-designer, devsecops and reviewer use `model_reasoning_effort = "high"`; the developers and tester use `"medium"`. To pin a model, uncomment the `model` line in each `.toml` file.
