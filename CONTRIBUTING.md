# Contributing to Agent Skills

Thank you for your interest in contributing to the AIBTC agent skills ecosystem. This document provides guidelines for submitting new skills, improving existing ones, and reporting issues.

## Quick Start

1. Fork the repository
2. Create a feature branch (`git checkout -b skill/my-new-skill`)
3. Make your changes
4. Submit a pull request

## Ways to Contribute

### Add a New Skill

Skills are self-contained prompt templates for AI agents. Each skill lives in its own directory with a `SKILL.md` file.

**Skill Structure:**
```
my-skill/
├── SKILL.md           # Required: Main skill definition
├── EXAMPLES.md        # Optional: Usage examples
└── README.md          # Optional: Extended documentation
```

**SKILL.md Format:**
```yaml
---
name: skill-name
description: "Brief description of what the skill does"
version: 1.0.0
user-invocable: true
metadata:
  tags: [tag1, tag2, tag3]
  beat: "Beat #N: Category" # Optional, if tied to a reporting beat
  source: "aibtc.news"      # Optional, source attribution
---
```

### Improve an Existing Skill

Improvements should:
- Maintain backward compatibility for agents already using the skill
- Add new instructions in the appropriate step/section
- Update version number in frontmatter
- Document changes in the commit message

### Fix Bugs or Typos

Found an error in documentation or skill logic?
- Open an issue describing the problem
- Or submit a PR with the fix

### Report Issues

Open a GitHub issue for:
- Incorrect or outdated instructions
- Missing error handling
- Broken tool references
- Integration problems with AIBTC MCP tools

## Skill Quality Standards

### Required Elements

Every skill must include:

1. **YAML Frontmatter** - Name, description, version, metadata
2. **Usage Instructions** - How to invoke the skill
3. **Step-by-Step Instructions** - Clear, numbered steps the agent should follow
4. **Output Format** - Example of expected output structure
5. **Error Handling** - Table of failure modes and recovery actions

### Recommended Elements

- **Examples** - Real usage scenarios
- **Tips** - Best practices and shortcuts
- **Limitations** - Known constraints or edge cases
- **Tool Dependencies** - List of required MCP tools

### Style Guidelines

- Use clear, imperative language ("Do X" not "You should do X")
- Keep steps atomic and sequentially ordered
- Include tool invocation examples for MCP tools
- Use tables for structured data (error handling, scoring rubrics)
- Use code blocks with language tags for API responses and commands

## Pull Request Process

1. **Branch Naming**
   - New skills: `skill/skill-name`
   - Improvements: `improve/skill-name`
   - Fixes: `fix/description`

2. **Commit Messages**
   ```
   Add skill-name: brief description
   
   Detailed description of what changed and why.
   
   Co-Authored-By: Agent Name <agent@example.com>
   ```

3. **PR Description**
   - What skill was added/modified
   - Why this change improves the ecosystem
   - How to test the skill
   - Any breaking changes

4. **Review Process**
   - PRs are reviewed for quality and alignment with ecosystem goals
   - Suggestions for improvement may be provided
   - Approved PRs are merged to `main`

## File Format Standards

### Markdown

- Use ATX-style headers (`#` for H1, `##` for H2, etc.)
- Use fenced code blocks with language tags
- Use tables for structured data
- Max line length: 100 characters (soft wrap)

### YAML Frontmatter

- Use double quotes for string values
- Use square brackets for arrays
- Keep descriptions under 100 characters

## Testing Your Contribution

Before submitting:

1. **Validate YAML** - Ensure frontmatter is valid YAML
2. **Check Tool References** - Verify MCP tool names match the AIBTC MCP server spec
3. **Test Instructions** - Walk through each step mentally to catch logical errors
4. **Verify Links** - Ensure any URLs or references are accessible

## Code of Conduct

- Be respectful and constructive
- Focus on the contribution, not the contributor
- Welcome questions and feedback
- Support newcomers to the ecosystem

## Questions?

Open an issue with the `question` label or reach out on the AIBTC network.

---

**License:** Public domain. Fork, improve, share.