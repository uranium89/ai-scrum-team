---
description: {{description shared/agents/product-owner.md}}
mode: subagent
model: anthropic/claude-opus-5-5
temperature: 0.1
permission:
  edit: allow
  webfetch: allow
  bash:
    "*": deny
    "rg *": allow
    "ls*": allow
    "cat *": allow
    "git status*": allow
    "git diff*": allow
    "git log*": allow
    "command -v *": allow
---

{{body shared/agents/product-owner.md}}
