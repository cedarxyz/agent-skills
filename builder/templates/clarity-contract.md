# Template: clarity-contract

Stacks smart contract with test harness.

## Files to Generate

### `contracts/{{name}}.clar`
```clarity
;; {{name}}
;; {{description}}

;; Constants
(define-constant ERR-NOT-AUTHORIZED (err u401))
(define-constant CONTRACT-OWNER tx-sender)

;; Data variables
(define-data-var contract-paused bool false)

;; Read-only functions
(define-read-only (get-info)
  (ok {
    name: "{{name}}",
    paused: (var-get contract-paused)
  })
)

;; Public functions
(define-public (set-paused (paused bool))
  (begin
    (asserts! (is-eq tx-sender CONTRACT-OWNER) ERR-NOT-AUTHORIZED)
    (var-set contract-paused paused)
    (ok true)
  )
)
```

### `tests/{{name}}.test.ts`
```typescript
import { describe, it, expect } from 'vitest';

describe('{{name}}', () => {
  it('should return contract info', () => {
    // Use simnet or clarinet for integration tests
    // This is a placeholder for unit test structure
    expect(true).toBe(true);
  });
});
```

### `Clarinet.toml`
```toml
[project]
name = "{{name}}"
authors = []

[contracts.{{name}}]
path = "contracts/{{name}}.clar"
```

### `settings/Devnet.toml`
```toml
[network]
name = "devnet"

[accounts.deployer]
mnemonic = "twice particular replace describe pagoda income opera shallow become vessel side prepare number panic jelly surgery finger diesel typing age group size peace write"
balance = 10_000_000_000_000
```

### `package.json`
```json
{
  "name": "{{name}}",
  "version": "0.1.0",
  "description": "{{description}}",
  "scripts": {
    "test": "vitest run",
    "check": "clarinet check"
  },
  "devDependencies": {
    "vitest": "^4.0.0"
  }
}
```

### `README.md`
```markdown
# {{name}}

{{description}}

Clarity smart contract for the [AIBTC](https://aibtc.com) ecosystem on Stacks.

## Setup

Install [Clarinet](https://github.com/hirosystems/clarinet):

```bash
brew install clarinet
```

## Check

```bash
clarinet check
```

## Test

```bash
clarinet test
```

## Deploy

Use the AIBTC MCP `deploy_contract` tool or deploy via Clarinet.
```

### `CLAUDE.md`
```markdown
# {{name}}

Clarity smart contract on Stacks.

## Commands

- `clarinet check` -- validate contract syntax
- `clarinet test` -- run tests
- `clarinet console` -- interactive REPL

## Rules

- All public functions must have authorization checks
- Use descriptive error constants (ERR-NOT-AUTHORIZED, etc.)
- Test every public function
```

### `.gitignore`
```
node_modules/
.cache/
```
