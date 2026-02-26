# Agent Skills

Free, open skills for autonomous AI agents in the AIBTC / Bitcoin-agent ecosystem. Each skill is a self-contained instruction set that agents can load directly via Claude Code, MCP server config, or as a reusable prompt template.

## Skills

| Skill | Beat | Description |
|-------|------|-------------|
| [reputation-intelligence](reputation-intelligence/SKILL.md) | #2 Reputation Intelligence | Research and evaluate trust signals before meaningful interactions. Aggregates on-chain identity, tx history, social signals, and ecosystem activity into a scored verdict. |
| [opportunity-board](opportunity-board/SKILL.md) | #3 Opportunity Board | Aggregate open bounties, yield opportunities, service gaps, and collaboration requests across the AIBTC ecosystem. The job board for agents with skills looking for money. |
| [moltbook-engage](moltbook-engage.md) | — | Engage with Moltbook to promote content, reply to comments, and find relevant posts. |
| [moltbook-promoter](moltbook-promoter.md) | — | Promote AIBTC content across Moltbook communities. |

## Installation

### Claude Code (local)

Copy a skill directory to your Claude Code skills folder:

```bash
# Single skill
cp -r reputation-intelligence ~/.claude/skills/reputation-intelligence

# All skills
cp -r */ ~/.claude/skills/
```

Then invoke with `/reputation-intelligence` or `/opportunity-board` in any session.

### As a prompt template

The `SKILL.md` files are self-contained prompt templates. Copy the content after the YAML frontmatter and use it as instructions for any LLM agent framework.

## Beats

These skills are designed to cover reporting beats on [aibtc.news](https://aibtc.news):

- **Beat #2: Reputation Intelligence** — "Who can I trust? Who's shipping? Who's dark?"
- **Beat #3: Opportunity Board** — "Open bounties, skill demand, projects needing help"

## Contributing

Fork, improve, share. If you spot new signals (emerging rep-gate APIs, new DeFi protocols, additional bounty boards), add them and open a PR.

## License

Public domain. Use however you want.
