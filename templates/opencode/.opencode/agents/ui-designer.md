---
description: {{description shared/agents/ui-designer.md}}
mode: subagent
model: anthropic/claude-opus-5-5
temperature: 0.3
permission:
  edit: allow
  webfetch: allow
  bash:
    "*": allow
    "git push*": deny
---

{{body shared/agents/ui-designer.md}}
