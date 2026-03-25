# Contributing to agent-skills

Free, open skills for autonomous AI agents in the AIBTC / Bitcoin-agent ecosystem. Contributions welcome — fork, improve, share.

## Quick Start

1. Fork the repo and clone it locally
2. Create a branch: `git checkout -b add-my-skill` or `git checkout -b fix-skill-name`
3. Make your changes
4. Commit with a conventional commit message
5. Push and open a PR

---

## Submitting a New Skill

### Directory Structure

Each skill lives in its own directory. The name must be **kebab-case** and describe what the skill does.

```
my-skill/
  SKILL.md         # Required — the skill definition
  AGENT.md         # Optional — autonomous operation rules for agents running this skill
  templates/       # Optional — template files referenced by the skill
```

Standalone skills (simpler, no templates) may also be a single Markdown file at the repo root:

```
my-skill.md        # Self-contained skill definition
```

### SKILL.md Format

Every `SKILL.md` requires a YAML frontmatter block at the top:

```yaml
---
name: my-skill
description: "One-line description of what this skill does."
version: 1.0.0
user-invocable: true
metadata:
  tags: [tag1, tag2, tag3]
  author: your-github-username
  author_agent: Your Agent Name
---
```

**Required frontmatter fields:**

| Field | Description |
|-------|-------------|
| `name` | Kebab-case skill name, matches directory name |
| `description` | One sentence describing the skill's purpose |
| `version` | Semantic version, start at `1.0.0` |
| `user-invocable` | `true` if agents can invoke it via slash command |
| `metadata.tags` | Relevant tags (array of strings) |

**Optional frontmatter fields:**

| Field | Description |
|-------|-------------|
| `metadata.author` | GitHub username of the contributor |
| `metadata.author_agent` | Agent name if contributed by an AI agent |
| `metadata.beat` | AIBTC news beat this skill covers (e.g., `"Beat #2: Reputation Intelligence"`) |

### SKILL.md Content Sections

After the frontmatter, structure your skill with these sections:

```markdown
# Skill Name

Brief description of what this skill does and why it exists.

## Usage

Invoke with: `/skill-name [arguments]`

- `/skill-name arg1` — What this does
- `/skill-name --flag` — What this does

## Instructions

<command-name>skill-name</command-name>

[Full agent instructions here — written in second person ("You are...", "Your job is...")]

### Step 0: Load Tools

[List MCP tools the skill needs]

### Step 1: ...

[Numbered steps the agent follows]
```

**Key conventions:**
- Use a `<command-name>skill-name</command-name>` tag right after `## Instructions` — this is how Claude Code identifies the active skill
- Write instructions as direct agent commands ("Fetch X", "Validate Y", "Output Z")
- Use numbered steps so agents can follow the flow deterministically
- Document output formats with JSON examples where applicable

### AGENT.md Format (Optional)

If your skill has complex autonomous behavior, add an `AGENT.md` with decision rules:

```markdown
# Skill Name -- Autonomous Operation Rules

Decision rules for autonomous operation when running this skill:

## Rules

1. **Always [X]** -- Reason
2. **Never [Y]** -- Reason

## Chaining Actions

[Describe how multi-step flows should be chained]

## Error Handling

[How the agent should handle failures]
```

### Naming Conventions

- **Directory names:** kebab-case, lowercase — `reputation-intelligence`, not `ReputationIntelligence`
- **Skill names in frontmatter:** match the directory name exactly
- **Tags:** lowercase, no spaces — `on-chain`, `bitcoin`, `defi`
- **No personal identifiers** in skill names or directory names — use project-descriptive names only

---

## Improving Existing Skills

1. Fork and clone the repo
2. Create a branch named after what you're changing: `git checkout -b improve-reputation-scoring`
3. Make the minimal change needed — one issue per PR
4. If you're adding a new signal source or data provider, document it in the skill's step instructions
5. Bump the `version` field in the frontmatter (patch for fixes, minor for new capabilities)
6. Open a PR with a clear description of what changed and why

**Good improvement candidates:**
- Adding new signal sources to existing research skills
- Improving output formats for better agent chaining
- Fixing broken API references or outdated endpoints
- Adding missing `AGENT.md` autonomous operation rules
- Improving step clarity or adding edge case handling

---

## Reporting Issues and Requesting Skills

**To report a bug in an existing skill:** Open an issue describing the unexpected behavior, which skill, and what you expected vs. what happened.

**To request a new skill:** Open an issue with:
- What the skill should do (one sentence)
- What AIBTC ecosystem problem it solves
- What MCP tools or APIs it would use
- Which AIBTC news beat it relates to (if any)

**To report a broken external dependency** (API changed, endpoint moved): Open an issue with the skill name, the broken reference, and a link to updated documentation if you have it.

---

## Quality Standards

Before opening a PR, check:

- [ ] YAML frontmatter is valid and all required fields are present
- [ ] Skill name in frontmatter matches the directory/filename
- [ ] Instructions are written clearly enough for an AI agent to follow without ambiguity
- [ ] Output formats include JSON examples where applicable
- [ ] No secrets, API keys, private keys, or mnemonics anywhere in the files
- [ ] Tags are lowercase and relevant
- [ ] If the skill calls external APIs, those APIs are real and accessible
- [ ] Version is bumped if modifying an existing skill

---

## Commit Conventions

Use conventional commits:

```
feat(skill-name): add new skill for X
fix(skill-name): correct broken API endpoint
docs(skill-name): improve step clarity
chore: update README skill table
```

Types: `feat`, `fix`, `docs`, `chore`, `refactor`

---

## PR Review Process

1. Open a PR against `main`
2. Describe what the skill does and why it's useful to the ecosystem
3. A maintainer will review for: format compliance, instruction clarity, and ecosystem fit
4. Address any feedback — reviews focus on making skills work well for agents, not style preferences
5. Once approved, it's merged and listed in README.md

PRs that add skills with clear ecosystem use cases and clean format are merged quickly. The bar is: "Would a real agent actually use this?"

---

## License

All contributions are released into the public domain. By submitting a PR you agree your contribution can be used freely by anyone.
