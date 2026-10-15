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

## The team

| Agent | Model | Job |
|---|---|---|
| product-owner | strong | Interviews you, writes user stories with acceptance criteria, plans sprints, accepts finished work, reports to you in plain language |
| architect | strong | Owns `ai-team/ARCHITECTURE.md` and ADRs, writes a technical spec for each story |
| coder | fast | Implements the spec and makes sure it builds |
| backend-dev, frontend-dev | fast | Replace `coder` when backend and frontend are separate codebases (fullstack mode, chosen at kickoff) |
| tester | fast | Writes and runs tests for every acceptance criterion |
| devsecops | strong | Runs security scanners as a blocking gate, writes threat models, owns CI and infrastructure config; never deploys |
| reviewer | strong | Reviews code and architecture conformance, challenges risky decisions, writes the sprint retro |

## What you do as the stakeholder

You never review code or architecture. You only:
1. **Answer business questions at kickoff**: users, budget, sensitive data, deadline, who maintains it.
2. **Approve each sprint plan** by replying "approved".
3. **Choose an option in decision briefs** (`ai-team/decisions/DEC-*.md`). These come only for 🔴 high-risk decisions and are written in cost, time and risk terms.
4. **Read the sprint review** (`ai-team/sprints/sprint-NN/review.md`), try the product with the steps it gives, and report any defects you find.

## How a sprint runs

```
Kickoff (once): business interview -> backlog -> architecture -> security gates
Sprint planning -> you approve
For each story:
  architect spec -> (devsecops threat model) -> coder -> tester -> devsecops gate -> reviewer -> product-owner acceptance
  NEEDS_FIX: Route CODER (small fix) or Route ARCHITECT (spec wrong), max 3 loops, then Blocked
Sprint review (for you) -> retro (proposed rule changes, you approve)
```

A story is Done only when build, lint, tests, the security gate, the reviewer and the product-owner all pass, each with evidence. For small changes, say `quick: <change>` to skip the sprint ceremonies; the gates still run.

## Risk levels

| Level | Example | Who decides |
|---|---|---|
| 🟢 | A small library, how one module is organized | The agent, recorded in the spec |
| 🟡 | A new service, a caching strategy | Architect proposes, reviewer challenges; escalated to you if they disagree |
| 🔴 | Database choice, login, payments, personal data, recurring cost, deleting data | You, through a decision brief; the story waits |

Before the first public release of anything with login, payments or personal data, the sprint review will say `EXPERT REVIEW NEEDED`. Hire a human security expert or senior engineer for a few hours at that point. The agents cannot fully replace that review.

## Caveats

- Role write zones are instructions to the model. Unless noted below, the tool does not enforce them.
- Security scanners catch known vulnerabilities and common patterns, not every flaw. Authorization and business-logic bugs depend on tests, review and the expert checkpoint.
- Running unattended needs a permission mode that skips approval prompts. Use git, and preferably a container, so you can undo changes. Agents are told never to deploy or push.
- 🔴 decisions and sprint approval stop and wait for you. Answer pending briefs before leaving the team running overnight.
- Cost: four agents use a strong model. Use `quick:` for small changes.

## Codex notes
- Codex only spawns subagents when asked explicitly, so the prompts say "Spawn subagent".
- Subagents share the main session's sandbox and approval policy; an agent file cannot restrict its own permissions. Set permissions for the main session with `/permissions`.
- Models: product-owner, architect, devsecops and reviewer use `model_reasoning_effort = "high"`; the developers and tester use `"medium"`. To pin a model, uncomment the `model` line in each `.toml` file.
