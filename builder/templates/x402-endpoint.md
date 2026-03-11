# Template: x402-endpoint

Paid API endpoint with x402 payment gate on Cloudflare Workers.

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
  // Add KV, D1, or other bindings here
};

const app = new Hono<{ Bindings: Bindings }>();

app.use('*', cors());

// Public endpoint -- no payment required
app.get('/', (c) => {
  return c.json({
    name: '{{name}}',
    description: '{{description}}',
    endpoints: {
      '/': 'This info page (free)',
      '/api/data': 'Paid endpoint (x402)',
    },
  });
});

// x402 payment-gated endpoint
// The x402 facilitator proxy handles payment verification
// before forwarding the request here. The request arrives
// with x-payer-address header set by the facilitator.
app.get('/api/data', (c) => {
  const payer = c.req.header('x-payer-address') || 'anonymous';

  // Your paid logic here
  return c.json({
    data: 'Your paid content here',
    paidBy: payer,
    timestamp: new Date().toISOString(),
  });
});

export default app;
```

### `README.md`
```markdown
# {{name}}

{{description}}

Built for the [AIBTC](https://aibtc.com) ecosystem.

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

## x402 Integration

This endpoint is designed to work with the x402 payment protocol. Register it with the AIBTC x402 facilitator to enable paid access.
```

### `CLAUDE.md`
```markdown
# {{name}}

x402 paid API endpoint on Cloudflare Workers.

## Build & Deploy

- `bun run dev` -- local development
- `bun run deploy` -- deploy to Cloudflare
- `bun run typecheck` -- check TypeScript types

## Architecture

- Hono web framework on Cloudflare Workers
- x402 payment gate via facilitator proxy
- Public info endpoint at `/`
- Paid endpoint at `/api/data`

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
