---
name: coder
description: {{description shared/agents/coder.md}}
tools: [view_file, grep_search, replace_file_content, run_command]
mainAgent: false
subagent: true
model: flash
commandExecutionPolicy: auto
---

# System Prompt

{{body shared/agents/coder.md}}
