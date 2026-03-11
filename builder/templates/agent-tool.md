# Template: agent-tool

New skill for the AIBTC ecosystem -- a markdown-based instruction set that agents can load and execute.

## Files to Generate

### `SKILL.md`
```markdown
---
name: {{name}}
description: "{{description}}"
version: 1.0.0
user-invocable: true
metadata:
  tags: []
---

# {{name}}

{{description}}

## Usage

Invoke with: `/{{name}} [action]`

Examples:
- `/{{name}}` -- Default action
- `/{{name}} help` -- Show usage

## Instructions

<command-name>{{name}}</command-name>

You are a {{name}} agent. {{description}}

Parse the user's input and execute the appropriate action.

---

### Step 0: Load Tools

Load MCP tools needed:

\`\`\`
ToolSearch query: "+aibtc"
\`\`\`

---

### Step 1: Default Action

Describe what the default action does and the steps to execute it.

**Output format (JSON):**
\`\`\`json
{
  "action": "default",
  "result": {}
}
\`\`\`
```

### `AGENT.md`
```markdown
# {{name}} -- Autonomous Operation Rules

1. Always validate inputs before executing actions
2. Output structured JSON for every action
3. Ask for confirmation before write operations
```

### `README.md`
```markdown
# {{name}}

{{description}}

Agent skill for the [AIBTC](https://aibtc.com) ecosystem.

## Installation

Copy to your Claude Code skills folder:

```bash
cp -r {{name}} ~/.claude/skills/{{name}}
```

Then invoke with `/{{name}}` in any session.

## Contributing

Fork, improve, share. Open a PR with your changes.
```
