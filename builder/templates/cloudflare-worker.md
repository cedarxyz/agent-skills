# Template: cloudflare-worker

General Cloudflare Worker with Hono, D1/KV bindings.

## Files to Generate

### `package.json`
```json
{
  "name": "{{name}}",
  "version": "0.1.0",
  "description": "{{description}}",
  "scripts": {
    "dev": "wrangler dev",
    "deploy": "wrangler deploy",
    "typecheck": "tsc --noEmit"
  },
  "dependencies": {
    "hono": "^4.7.0"
  },
  "devDependencies": {
    "@cloudflare/workers-types": "^4.20250214.0",
    "typescript": "^5.7.0",
    "wrangler": "^4.0.0"
  }
}
```

### `wrangler.toml`
```toml
name = "{{name}}"
main = "src/index.ts"
compatibility_date = "2025-01-01"
compatibility_flags = ["nodejs_compat"]

# Uncomment to add D1 database
# [[d1_databases]]
# binding = "DB"
# database_name = "{{name}}-db"
# database_id = ""

# Uncomment to add KV namespace
# [[kv_namespaces]]
# binding = "KV"
# id = ""
```

### `tsconfig.json`
```json
{
  "compilerOptions": {
    "target": "ESNext",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "outDir": "dist",
    "types": ["@cloudflare/workers-types"]
  },
  "include": ["src"]
}
```

### `src/index.ts`
```typescript
import { Hono } from 'hono';
import { cors } from 'hono/cors';

type Bindings = {
  // DB: D1Database;
  // KV: KVNamespace;
};

const app = new Hono<{ Bindings: Bindings }>();

app.use('*', cors());

app.get('/', (c) => {
  return c.json({
    name: '{{name}}',
    description: '{{description}}',
    status: 'ok',
  });
});

app.get('/api/health', (c) => {
  return c.json({ status: 'healthy', timestamp: new Date().toISOString() });
});

export default app;
```

### `README.md`
```markdown
# {{name}}

{{description}}

Cloudflare Worker built for the [AIBTC](https://aibtc.com) ecosystem.

## Setup

```bash
bun install
```

## Development

```bash
bun run dev
```

## Deploy

```bash
bun run deploy
```

## Bindings

Uncomment D1/KV sections in `wrangler.toml` and create resources:

```bash
# Create D1 database
wrangler d1 create {{name}}-db

# Create KV namespace
wrangler kv namespace create KV
```
```

### `CLAUDE.md`
```markdown
# {{name}}

Cloudflare Worker with Hono.

## Build & Deploy

- `bun run dev` -- local development
- `bun run deploy` -- deploy to Cloudflare
- `bun run typecheck` -- check types

## Rules

- Never include personal identifiers in the worker name
- Keep the worker name as: {{name}}
```

### `.gitignore`
```
node_modules/
dist/
.wrangler/
.dev.vars
```
