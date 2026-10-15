# AI Scrum Team Templates

An AI Scrum team that builds software for a non-technical stakeholder: a product-owner, architect, coder (or backend and frontend devs), tester, devsecops and reviewer hand work to each other through files. Risky decisions come to you as plain-language decision briefs, and a story is Done only after tests, a security gate, code review and acceptance all pass. Packaged for five AI coding tools.

## Use a template

Pick your tool, then copy the contents of its `dist/` folder, including hidden folders, into your project root. Each folder has its own README with the kickoff, sprint and quick-change prompts, and an explanation of what you do as the stakeholder.

| Tool | Copy from | Rules file | Agents |
|---|---|---|---|
| Claude Code | `dist/claude/` | `CLAUDE.md` | `.claude/agents/*.md` |
| OpenAI Codex | `dist/codex/` | `AGENTS.md` | `.codex/agents/*.toml` |
| Cursor | `dist/cursor/` | `.cursor/rules/ai-team.mdc` | `.cursor/agents/*.md` |
| OpenCode | `dist/opencode/` | `AGENTS.md` | `.opencode/agents/*.md` |
| Google Antigravity | `dist/antigravity/` | `AGENTS.md` | `.agents/agents/*.md` |

```sh
cp -R dist/claude/. /path/to/your-project/
```

Recommended models: a strong model (e.g. Opus) for product-owner, architect, devsecops and reviewer, and a fast, cheaper model (e.g. Sonnet) for coder, backend-dev, frontend-dev and tester. Each template sets this up in its agent files.

## Repository layout

```
shared/       content common to every tool (edit here first)
  RULES.md      roles, hard limits, kickoff, sprint workflow, risk process, definition of done, file formats
  agents/       the eight agent prompts (description + body)
  docs/         README sections included in every tool's README
  project/      backlog/ stories/ sprints/ decisions/ docs/ skeleton + example story
templates/    tool-specific parts only: front matter, file formats, orchestration notes
dist/         generated, ready-to-copy templates (do not edit by hand)
scripts/build.py
```

## Edit a template

1. Change shared content in `shared/`, and tool-specific content in `templates/<tool>/`.
2. Run `python3 scripts/build.py` to regenerate `dist/`.
3. Run `python3 scripts/build.py --check` to confirm `dist/` is up to date.

Files in `templates/` reference shared content with directives that the build expands:

| Directive | Expands to |
|---|---|
| `{{include PATH}}` | the whole file |
| `{{body PATH}}` | a shared agent prompt without its front matter |
| `{{description PATH}}` | the `description:` value from a shared agent's front matter |

To add a tool, create `templates/<tool>/` with its rules file, one agent file per `shared/agents/*.md` and a README, then rebuild. To add a role, add `shared/agents/<role>.md`, a wrapper in every `templates/<tool>/` agents folder, and the role to `shared/RULES.md` and `shared/docs/WORKFLOW.md`.
## License

[MIT](LICENSE)
