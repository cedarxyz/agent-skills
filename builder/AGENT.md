# Builder Agent -- Autonomous Operation Rules

Decision rules for autonomous operation when running the builder skill:

## Rules

1. **Always start with `templates`** -- Before scaffolding, list templates to pick the right one for the task. Match the template to the project requirements.

2. **Always run `check` before deployment** -- Never deploy without passing all validation checks first. Fix any blockers before proceeding.

3. **Never deploy without consent** -- Deployment is a write operation visible to others. Always ask the user/operator for explicit confirmation before running `wrangler deploy` or any deployment command.

4. **Suggest `list-project` after deploy** -- After a successful deployment, suggest registering the project on the AIBTC projects board. Don't auto-register without confirmation.

5. **Use branding for all frontend projects** -- Any project with a UI (dashboard, static-site, x402-endpoint with a landing page) must use AIBTC design tokens. Run `branding` to get the tokens and apply them.

6. **Validate project names** -- Always check project names against the personal identifier rule: never include personal names like "derek-pym" in Cloudflare URLs or project names. Use project-specific names only.

7. **Prefer single-file for simple projects** -- If the project is simple enough (dashboard, landing page), prefer a single HTML file over a complex build setup. Only add build tooling when the project genuinely needs it.

8. **Don't over-scaffold** -- Generate the minimum files needed. A scaffold is a starting point, not a finished product. Prefer fewer, well-structured files over many boilerplate files.

9. **Git init but don't commit** -- Initialize a git repo in scaffolded projects but let the developer make the first commit after reviewing the generated code.

10. **Template files are references** -- Read template files from the builder/templates/ directory. If a template file doesn't exist for a requested type, generate reasonable defaults based on the template catalog in SKILL.md.

## Chaining Actions

When running the full pipeline autonomously:

```
templates -> scaffold -> [develop] -> check -> deploy -> list-project
```

- Pause after scaffold to let the developer customize
- Pause after check if there are warnings (not just blockers)
- Always pause before deploy for confirmation
- Pause before list-project for confirmation

## Error Handling

- If `wrangler` is not installed: suggest `npm install -g wrangler` and stop
- If `clarinet` is not installed for Clarity projects: skip the syntax check, note it as a warning
- If the projects API is unreachable: save the listing payload locally and suggest manual submission
- If scaffold target directory already exists: warn and ask before overwriting
