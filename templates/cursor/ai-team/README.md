# AI Scrum Team for Cursor

## Setup
1. Copy everything in the template folder (`dist/cursor/`, the parent of this `ai-team/` folder), including the hidden `.cursor/`, into your project root.
2. The rules live in `.cursor/rules/ai-team.mdc` (`alwaysApply: true`, so they are always loaded). The subagents live in `.cursor/agents/`.

## Prompts (in Agent chat)
**1. Kickoff** (once per project):
```
Kick off a new product: [WHAT YOU WANT, in your own words].
Act as the orchestrator defined in .cursor/rules/ai-team.mdc and run the Kickoff.
Invoke each agent with its slash command (/product-owner, /architect, ...), one at a time.
Ask me the business interview questions and wait for my answers. Then finish the Kickoff and show me the backlog, any decision briefs and the first sprint plan.
```

**2. Run a sprint** (after you reply "approved" to the plan):
```
Run sprint [NN] as defined in .cursor/rules/ai-team.mdc, story by story.
Invoke each agent with its slash command (/product-owner, /architect, ...), one at a time.
Stop when the sprint is finished or nothing can proceed, then show me the sprint review.
```

**3. Quick change** (small, low-risk):
```
quick: [the change]
```

{{include shared/docs/WORKFLOW.md}}

## Cursor notes
- Models: product-owner, architect, ui-designer, devsecops and reviewer use `claude-opus-5[effort=high]`; the developers and tester use `composer-2`. If your plan lacks a model, Cursor falls back to a compatible one; set `model: inherit` to use the main session's model. Setting the reviewer to a different vendor's model reduces shared blind spots.
- Every agent has `readonly: false` because each must write its own output file, so write zones are only instructions.
- Cursor also reads `.claude/agents/`. If the project has both, `.cursor/agents/` wins on name conflicts.
- Don't keep other Markdown files in subfolders of `.cursor/agents/`: a known Cursor bug detects them as subagents.
