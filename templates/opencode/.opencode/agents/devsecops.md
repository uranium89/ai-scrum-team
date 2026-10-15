---
description: {{description shared/agents/devsecops.md}}
mode: subagent
model: anthropic/claude-opus-5-5
temperature: 0.1
permission:
  edit: allow
  webfetch: allow
  bash:
    "*": ask
    "rg *": allow
    "ls*": allow
    "cat *": allow
    "git status*": allow
    "git diff*": allow
    "git log*": allow
    "command -v *": allow
    "osv-scanner *": allow
    "gitleaks *": allow
    "semgrep *": allow
    "trivy *": allow
    "npm audit*": allow
    "pip-audit*": allow
    "git push*": deny
---

{{body shared/agents/devsecops.md}}
