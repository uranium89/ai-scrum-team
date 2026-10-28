# AI Scrum Team for Claude Code

## Setup
1. Copy everything in the template folder (`dist/claude/`, the parent of this `ai-team/` folder), including the hidden `.claude/`, into your project root.
2. Claude Code loads `CLAUDE.md` and the subagents in `.claude/agents/` automatically. Run `/agents` to confirm all nine are listed.

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

## The team

| Agent | Model | Job |
|---|---|---|
| product-owner | strong | Interviews you, writes user stories with acceptance criteria, plans sprints, accepts finished work, reports to you in plain language |
| architect | strong | Owns `ai-team/ARCHITECTURE.md` and ADRs, writes a technical spec for each story |
| coder | fast | Implements the spec and makes sure it builds |
| backend-dev, frontend-dev | fast | Replace `coder` when backend and frontend are separate codebases (fullstack mode, chosen at kickoff) |
| ui-designer | strong | On for any product with a user interface: proposes visual styles for you to choose, owns `ai-team/DESIGN.md`, writes a UI spec per story, and checks the built screens from screenshots |
| tester | fast | Writes and runs tests for every acceptance criterion |
| devsecops | strong | Runs security scanners as a blocking gate, writes threat models, owns CI and infrastructure config; never deploys |
| reviewer | strong | Reviews code and architecture conformance, challenges risky decisions, writes the sprint retro |

## What you do as the stakeholder

You never review code or architecture. You only:
1. **Answer business questions at kickoff**: users, budget, sensitive data, deadline, who maintains it, and for a product with screens, apps or websites whose look you like and your brand colors.
2. **Approve each sprint plan** by replying "approved".
3. **Choose an option in decision briefs** (`ai-team/decisions/DEC-*.md`). These come only for 🔴 high-risk decisions and are written in cost, time and risk terms. For a product with screens, the first brief asks you to pick a visual style from 2-3 mockups.
4. **Read the sprint review** (`ai-team/sprints/sprint-NN/review.md`), look at the screenshots, try the product with the steps it gives, and report any defects you find, including anything that looks wrong to you.

## How a sprint runs

```
Kickoff (once): business interview -> backlog -> architecture -> security gates -> visual style (you choose) -> design system
Any request (feature, change or bug) -> product-owner writes a user story in the backlog first
Sprint planning -> you approve
For each story:
  architect spec -> (ui-designer UI spec) -> (devsecops threat model) -> coder -> tester -> (ui-designer visual check) -> devsecops gate -> reviewer -> product-owner acceptance
  NEEDS_FIX: Route CODER (small fix) or Route ARCHITECT (spec wrong), max 3 loops, then Blocked
Sprint review (for you) -> retro (proposed rule changes, you approve)
```

A story is Done only when build, lint, tests, the visual check (for screens), the security gate, the reviewer and the product-owner all pass, each with evidence. For small changes, say `quick: <change>` to skip the sprint ceremonies; the gates still run. Every feature request or bug you report becomes a user story in the backlog first, even in quick mode, so nothing gets lost and you can see what is queued.

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
- The ui-designer makes the UI clean and consistent, not a unique brand. If the brand matters to your business, hire a human designer for a few hours to review `ai-team/DESIGN.md` or give brand guidelines; the team follows them. For UI-heavy products you can also switch `frontend-dev` (or `coder`) to a strong model.
- The visual check needs a tool that can view images. If yours can't, the ui-designer reports it as a gap instead of passing screens it hasn't seen.
- Cost: four agents use a strong model, five with ui-designer on. Use `quick:` for small changes.

## Claude Code notes
- Models: product-owner, architect, ui-designer, devsecops and reviewer use `model: opus`; coder, backend-dev, frontend-dev and tester use `model: sonnet`. These aliases resolve to the newest model in each family.
- Agents have every tool available. To restrict one, add a `tools:` list to its front matter.
- For a reviewer from a different vendor, run the review step in another tool (for example the Codex or OpenCode template) on the same repo.
