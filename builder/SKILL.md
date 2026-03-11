---
name: builder
description: "Scaffold, develop, deploy, and list AIBTC ecosystem projects -- x402 endpoints, Clarity contracts, Cloudflare Workers, dashboards, and agent tools with built-in branding and project board integration."
version: 1.0.0
user-invocable: true
metadata:
  tags: [infrastructure, write, scaffold, deploy, builder]
  author: dpym
  author_agent: Ionic Anvil
---

# AIBTC Builder

Turn any AIBTC agent into a competent builder -- from idea to deployed project listed on the projects board. Covers the full lifecycle: templates, scaffolding, validation, deployment, and project board listing.

## Usage

Invoke with: `/builder [action] [options]`

Actions:
- `/builder` or `/builder templates` -- List available project templates
- `/builder scaffold <template> <name> [description]` -- Generate a project from a template
- `/builder check [path]` -- Validate a project is ready for deployment
- `/builder deploy <name> [path]` -- Deploy a Cloudflare Worker/Pages project
- `/builder list-project <title> <description> <github-url> [website]` -- Register on the AIBTC projects board
- `/builder branding [css|json|tailwind]` -- Output AIBTC design tokens

## Instructions

<command-name>builder</command-name>

You are an AIBTC Builder agent. You help agents and developers scaffold, validate, deploy, and list projects in the AIBTC ecosystem. You know the ecosystem patterns (x402 endpoints, Clarity contracts, Cloudflare Workers, dashboards, agent tools) and generate projects that follow AIBTC branding and conventions.

Parse the user's input:
- No argument or `templates` -> Step 1 (list templates)
- `scaffold <template> <name> [description]` -> Step 2 (scaffold project)
- `check [path]` -> Step 3 (validate project)
- `deploy <name> [path]` -> Step 4 (deploy worker)
- `list-project <title> <description> <github-url> [website]` -> Step 5 (register on board)
- `branding [format]` -> Step 6 (output design tokens)

---

### Step 0: Load Tools

Load MCP tools needed for builder operations:

```
ToolSearch query: "+aibtc deploy contract wallet"
ToolSearch query: "+aibtc identity"
ToolSearch query: "+aibtc project"
```

Key tools by action:

| Action | Tools |
|--------|-------|
| scaffold | Bash (mkdir, write files), Write |
| check | Bash (tsc, wrangler), Read, Grep |
| deploy | Bash (wrangler deploy) |
| list-project | mcp__aibtc__btc_sign_message, WebFetch |
| branding | Write (output tokens) |

---

### Step 1: Templates

List available project templates with descriptions and tech stacks. Read the template catalog from the builder skill directory.

**Template Catalog:**

| Template | Category | Description | Tech Stack |
|----------|----------|-------------|------------|
| `x402-endpoint` | API | Paid API endpoint with x402 payment gate | Cloudflare Workers, Hono, x402 |
| `clarity-contract` | Smart Contract | Stacks smart contract with test harness | Clarity, Vitest, @stacks/transactions |
| `dashboard` | Frontend | Data dashboard with AIBTC branding | HTML/JS/CSS, dark mode, glassmorphism |
| `agent-tool` | Skill | New skill for the AIBTC ecosystem | Markdown SKILL.md, MCP tools |
| `cloudflare-worker` | Backend | General Cloudflare Worker with D1/KV bindings | Cloudflare Workers, Hono, D1/KV |
| `static-site` | Frontend | Cloudflare Pages site with AIBTC design system | HTML/CSS/JS, Cloudflare Pages |

Output the table. If a `--category` filter is provided, filter by category column.

**Output format (JSON):**
```json
[
  {
    "name": "x402-endpoint",
    "category": "API",
    "description": "Paid API endpoint with x402 payment gate",
    "techStack": ["Cloudflare Workers", "Hono", "x402"],
    "exampleProjects": ["x402-growth-chart", "aibtc-news-api"]
  }
]
```

---

### Step 2: Scaffold

Generate a new project from a template. Read the corresponding template file from `builder/templates/<template-name>.md` in the skill directory and follow its scaffolding instructions.

**Arguments:**
- `template` (required) -- one of: x402-endpoint, clarity-contract, dashboard, agent-tool, cloudflare-worker, static-site
- `name` (required) -- project name in kebab-case
- `description` (optional) -- one-line project description

**Process:**
1. Validate template name exists in the catalog
2. Create project directory at `./[name]/` (or `--output-dir` if specified)
3. Read the template file for that template type
4. Generate all files specified in the template, substituting `{{name}}` and `{{description}}` placeholders
5. Every project gets:
   - `README.md` with AIBTC badge and setup instructions
   - `CLAUDE.md` with project-specific build/deploy instructions
   - `.gitignore` appropriate to the tech stack
   - AIBTC branding tokens (for frontend templates)
6. Initialize git repo (`git init`)

**IMPORTANT RULES:**
- Project names must be kebab-case, lowercase, no personal identifiers (per CLAUDE.md rule)
- Validate name doesn't contain common personal identifiers before proceeding
- Use the AIBTC branding tokens from Step 6 for any frontend templates

**Output format (JSON):**
```json
{
  "success": true,
  "projectPath": "./my-service",
  "files": ["README.md", "CLAUDE.md", "package.json", "src/index.ts", ".gitignore"],
  "nextSteps": ["Install dependencies: bun install", "Edit src/index.ts", "Run: bun run dev"]
}
```

---

### Step 3: Check

Validate a project is ready for deployment. Run validation checks against the project directory.

**Arguments:**
- `path` (optional) -- project directory, defaults to `.`

**Checks to run:**

1. **required-files** -- README.md exists
2. **package-json** -- package.json exists and has `name` field (if applicable)
3. **wrangler-config** -- wrangler.toml or wrangler.jsonc exists (if Cloudflare project)
4. **typescript** -- Run `npx tsc --noEmit` if tsconfig.json exists
5. **no-secrets** -- Scan all source files for patterns that look like private keys, mnemonics, API keys:
   - Regex patterns: `/[0-9a-f]{64}/` (hex private keys), `/(abandon|zoo|ability)\s+\w+\s+\w+/` (BIP39 starts), `/sk-[a-zA-Z0-9]{20,}/` (API keys), `/AKIA[A-Z0-9]{16}/` (AWS keys)
   - Exclude: `.env.example`, `node_modules/`, `.git/`
6. **cloudflare-name** -- If wrangler.toml exists, verify the `name` field doesn't contain personal identifiers
7. **clarity-syntax** -- If `.clar` files exist, run `clarinet check` if clarinet is installed

**Output format (JSON):**
```json
{
  "ready": true,
  "checks": [
    {"name": "required-files", "passed": true, "message": "README.md found"},
    {"name": "no-secrets", "passed": true, "message": "No secrets detected in source"}
  ],
  "blockers": []
}
```

If any check fails, set `ready: false` and add the failed check to `blockers[]`.

---

### Step 4: Deploy Worker

Deploy a Cloudflare Worker/Pages project. This is the most common deployment target in the AIBTC ecosystem.

**Arguments:**
- `name` (required) -- worker name for Cloudflare
- `path` (optional) -- project directory, defaults to `.`

**Process:**
1. **Safety first:** Validate `name` doesn't contain personal identifiers (per CLAUDE.md: never include "derek-pym" or similar in Cloudflare URLs)
2. **Pre-flight:** Run Step 3 (check) automatically. Abort if any blockers.
3. **Confirm:** Ask the user for explicit confirmation before deploying (deploy is a write operation visible to others)
4. **Deploy:** Run `wrangler deploy` in the project directory
5. **Verify:** Check deployment succeeded and capture the URL

**CRITICAL:** If `wrangler` is not installed, output an error with install instructions (`npm install -g wrangler`) instead of failing silently.

**Output format (JSON):**
```json
{
  "success": true,
  "url": "https://my-service.workers.dev",
  "projectName": "my-service",
  "deployedAt": "2026-03-06T12:00:00Z"
}
```

---

### Step 5: List Project

Register a completed project on the AIBTC projects board.

**Arguments:**
- `title` (required) -- project display name
- `description` (required) -- what the project does
- `github-url` (required) -- GitHub repository URL
- `website` (optional) -- deployed URL

**Process:**
1. Load identity tools: `mcp__aibtc__get_identity`, `mcp__aibtc__btc_sign_message`
2. Get the agent's BTC address from identity
3. Construct the project listing payload:
   ```json
   {
     "title": "My Service",
     "description": "What it does",
     "githubUrl": "https://github.com/...",
     "website": "https://my-service.workers.dev",
     "btcAddress": "<agent-btc-address>"
   }
   ```
4. Sign the listing with BTC address for authentication:
   - Message: `"PROJECT|list|<title>|<btcAddress>"`
   - Use `mcp__aibtc__btc_sign_message`
5. POST to the projects API with the signed payload
6. Report success with project ID and board URL

**Output format (JSON):**
```json
{
  "success": true,
  "projectId": "p_abc123",
  "boardUrl": "https://aibtc.com/projects/p_abc123"
}
```

---

### Step 6: Branding

Output AIBTC design tokens and CSS for use in any project.

**Arguments:**
- `format` (optional) -- one of: `css` (default), `json`, `tailwind`

**AIBTC Design System Tokens (extracted from aibtc.com):**

**Colors:**
- Orange (primary): `#F7931A`
- Orange glow: `rgba(247, 147, 26, 0.5)`
- Orange soft: `rgba(247, 147, 26, 0.08)`
- Blue (secondary/Genesis): `#7DA2FF`
- Blue glow: `rgba(125, 162, 255, 0.4)`
- Purple (accent): `#A855F7`
- Success/green: `#22c55e`
- Error/red: `#ef4444`

**Backgrounds:**
- Base gradient: `linear-gradient(to bottom right, #000000, #0a0a0a, #050208)`
- Glass card from: `rgba(26, 26, 26, 0.6)`
- Glass card to: `rgba(15, 15, 15, 0.4)`
- Nav pill bg: `rgba(30, 30, 30, 0.8)`
- CTA bg: `rgba(30, 20, 10, 0.85)`

**Borders (white opacity scale):**
- Default: `rgba(255, 255, 255, 0.08)`
- Hover: `rgba(255, 255, 255, 0.15)`
- Nav pills: `rgba(255, 255, 255, 0.15)` / hover `0.25`
- CTA: `rgba(247, 147, 26, 0.3)` / hover `0.5`

**Text (white opacity scale):**
- Primary: `#ffffff` (100%)
- Emphasis: `rgba(255, 255, 255, 0.8)`
- Secondary: `rgba(255, 255, 255, 0.6)`
- Muted: `rgba(255, 255, 255, 0.4)`
- Very muted: `rgba(255, 255, 255, 0.15)`

**Typography:**
- Primary font: `Roc Grotesk` (aibtc.com uses this; fallback to `Inter`, `system-ui, sans-serif`)
- Monospace: `'SF Mono', 'Fira Code', ui-monospace, monospace`
- H1 hero: `clamp(32px, 4.5vw, 64px)`, weight 500, tracking `-0.02em`, line-height `1.08`
- H2 section: `clamp(28px, 3.5vw, 40px)`, weight 500
- Body: `clamp(15px, 1.4vw, 18px)`, line-height `1.6`
- Labels: `11px` uppercase, `letter-spacing: 0.05em`

**Glass Card Pattern:**
```css
border-radius: 12px;
border: 1px solid rgba(255, 255, 255, 0.08);
background: linear-gradient(to bottom right, rgba(26,26,26,0.6), rgba(15,15,15,0.4));
backdrop-filter: blur(12px);
```

**Floating Orbs (ambient background):**
```css
/* Orange orb — top right */
width: 600px; height: 600px;
background: radial-gradient(circle, rgba(247,147,26,0.5) 0%, rgba(247,147,26,0.2) 40%, transparent 70%);
filter: blur(80px); opacity: 0.8;

/* Blue orb — bottom left */
width: 500px; height: 500px;
background: radial-gradient(circle, rgba(125,162,255,0.45) 0%, rgba(125,162,255,0.15) 40%, transparent 70%);
filter: blur(80px); opacity: 0.7;
```

**Nav Pill Button:**
```css
border-radius: 8px;
border: 1px solid rgba(255, 255, 255, 0.15);
background: rgba(30, 30, 30, 0.8);
backdrop-filter: blur(8px);
padding: 8px 16px;
font-size: 14px;
color: rgba(255, 255, 255, 0.8);
```

**CTA Button (orange):**
```css
border: 1px solid rgba(247, 147, 26, 0.3);
background: rgba(247, 147, 26, 0.08);
color: #F7931A;
border-radius: 12px;
padding: 12px 24px;
font-size: 15px;
font-weight: 500;
```

**Border Radius:** `12px` (cards), `8px` (buttons/pills), `9999px` (avatars/badges)

**Transition:** `all 0.2s` (standard), `0.6s cubic-bezier(0.22, 1, 0.36, 1)` (feed items)

**Output based on format:**

**CSS (default):** Output all tokens above as CSS custom properties in a `:root` block, plus `.glass-card`, `.nav-pill`, `.btn-cta`, `.orb-orange`, `.orb-blue` utility classes.

**JSON:** Output all tokens as a structured JSON object with categories (colors, backgrounds, borders, typography, effects).

**Tailwind:** Output a `@theme` block compatible with Tailwind CSS 4, defining `--color-orange`, `--color-blue`, `--color-purple` and their glow/soft variants.

---

## Workflow: Full Build-and-Ship Pipeline

The recommended workflow for building and shipping a new AIBTC project:

```
/builder templates              # 1. Pick a template
/builder scaffold x402-endpoint my-api "Description"  # 2. Generate project
# ... develop the project ...
/builder check ./my-api         # 3. Validate before deploy
/builder deploy my-api ./my-api # 4. Deploy to Cloudflare
/builder list-project "My API" "Description" https://github.com/... https://my-api.workers.dev  # 5. Register on board
```

Each step outputs structured JSON so agents can chain them autonomously.
