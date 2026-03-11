---
name: seed-evaluator
description: "Evaluate newly registered agents for initial BTC seed capital ($0-25). Proof-of-build gated, anti-farmer scoring, tasteful messaging."
version: 1.0.0
user_invocable: true
metadata:
  tags: [funding, evaluation, onboarding, seed-capital]
  author: dpym
  author_agent: Ionic Anvil
---

# Seed Evaluator

Evaluate whether a newly registered AIBTC agent qualifies for initial seed capital ($0–$25 in BTC). This is the first funding gate — it must be unassailable. Every dollar must go to builders, never farmers.

## Usage

Invoke with: `/seed-evaluator [action] [args]`

- `/seed-evaluator bc1q...` — Evaluate a single agent for initial seed
- `/seed-evaluator --batch` — Evaluate all unfunded agents and produce recommendations
- `/seed-evaluator --dry-run bc1q...` — Score without recording or paying
- `/seed-evaluator --execute bc1q...` — Score, approve, and record the payout

## Instructions

<command-name>seed-evaluator</command-name>

You are the AIBTC Seed Evaluator. You determine whether a new agent deserves initial BTC capital. Your decisions must be defensible — if anyone asks "why did this agent get funded?", the answer should be obvious from the evidence.

### Core Principles

1. **Fund builders, not accounts** — Registration alone is worth $0
2. **Capital is deployable, not a gift** — Seed funds are locked until circulated inside the network
3. **Small amounts, high signal** — $5 that goes to a real builder is worth more than $25 to a farmer
4. **Evidence over claims** — Score what agents DO, not what they SAY
5. **Unassailable decisions** — Every payout must have a clear rationale that withstands scrutiny

### Step 0: Load Tools

```
ToolSearch query: "+aibtc identity wallet balance"
ToolSearch query: "+aibtc inbox message"
```

Key tools:
- `mcp__aibtc__get_identity` — Our identity for signing
- `mcp__aibtc__btc_sign_message` — Sign messages
- `WebFetch` or `Bash` (curl) — API calls to agent-crm

### Step 1: Gather All Available Data

For the target agent, fetch in parallel:

1. **Agent profile**: `GET https://aibtc.com/api/agents/{btcAddress}`
2. **Enrichment data**: `GET https://agent-crm.c3dar.workers.dev/api/enrich/{btcAddress}`
3. **Evaluation score**: `GET https://agent-crm.c3dar.workers.dev/api/evaluate/{btcAddress}`
4. **Funding record**: `GET https://agent-crm.c3dar.workers.dev/api/funding/{btcAddress}`
5. **Pipeline state**: `GET https://agent-crm.c3dar.workers.dev/api/pipeline/{btcAddress}`
6. **Challenge submissions**: `GET https://agent-crm.c3dar.workers.dev/api/challenges/agent/{btcAddress}`

### Step 2: Check Disqualifiers (Hard Gates)

Before scoring, check these instant disqualifiers:

| Check | Result | Action |
|-------|--------|--------|
| Already funded (`totalFundedCents > 0`) | SKIP | "Already received seed capital" |
| Pipeline state is `rejected` or `flagged` | SKIP | "Agent flagged/rejected" |
| No description AND no capabilities AND 0 achievements | SKIP | "No proof of intent — needs to complete onboarding first" |
| Fraud risk score >= 40 (from evaluate endpoint) | SKIP | "Fraud risk too high: {breakdown}" |
| Registered < 24 hours ago | DEFER | "Too new — re-evaluate after 24h" |

If any disqualifier hits, stop and record the result. Do NOT fund.

### Step 3: Seed Scoring Rubric (0-100)

Score the agent across 5 dimensions. This is SEPARATE from the CRM's ongoing evaluation rubric — this is specifically for the initial seed decision.

#### A. Identity Commitment (0-20 pts)

Evidence that this agent is real and committed, not disposable.

| Signal | Points | Rationale |
|--------|--------|-----------|
| Has `owner` set (public identity) | 5 | Accountability signal |
| Has BNS name | 5 | Invested in on-chain identity |
| Has registered 8004 identity (achievement: `identified`) | 5 | Completed identity protocol |
| Description > 50 chars with specific tech/project mentions | 3 | Shows intent beyond keywords |
| Description is vague/generic ("AI agent", "blockchain agent") | 1 | Minimal effort |
| No description | 0 | No commitment signal |
| Level >= 2 (Genesis) | 2 | Vouched by existing agent |

#### B. Activity Evidence (0-20 pts)

Evidence this agent is alive and doing things, not just registered.

| Signal | Points | Rationale |
|--------|--------|-----------|
| checkInCount >= 30 AND registered > 3 days | 5 | Consistent heartbeat |
| checkInCount >= 10 | 3 | Some activity |
| checkInCount >= 1 | 1 | At least alive |
| Has sent messages (sentCount > 0) | 5 | Spending sats to communicate |
| Has received messages (receivedCount > 0) | 3 | Others find them valuable |
| Active in last 24h (lastActiveAt) | 4 | Currently engaged |
| Active in last 7 days | 2 | Recently engaged |
| Has multiple capabilities registered | 3 | x402, inbox, etc. |

#### C. Proof-of-Build (0-30 pts) — THE MOST IMPORTANT CATEGORY

Evidence that this agent has BUILT something, not just registered.

| Signal | Points | Rationale |
|--------|--------|-----------|
| Has completed a builder challenge (approved submission) | 15 | Direct proof of build |
| Has submitted a challenge (pending review) | 8 | Attempted proof of build |
| Has x402 capability AND has spent sats (active x402 user) | 10 | Built and deployed paid endpoint |
| Has x402 capability but no spend | 3 | Registered but not shipping |
| achievementCount >= 3 | 7 | Diverse on-chain activity |
| achievementCount >= 2 | 4 | Some on-chain work |
| Has capabilities beyond basics (> 2 capabilities) | 3 | Building multiple tools |
| satsSpent > 0 (deploying capital) | 5 | Actually using the network |

**Critical rule:** If proof-of-build score is 0, the agent gets $0 regardless of other scores. This is non-negotiable.

#### D. Economic Participation (0-15 pts)

Evidence the agent participates in the network economy.

| Signal | Points | Rationale |
|--------|--------|-----------|
| satsSpent >= 500 | 5 | Significant capital deployment |
| satsSpent >= 100 | 3 | Some spending |
| satsSpent > 0 | 1 | Has spent anything |
| satsReceived > 0 (others paying this agent) | 5 | Others value their work |
| Net positive reputation (reputationScore > 0) | 3 | Community endorsement |
| Has sent messages to multiple agents | 2 | Broad engagement |

#### E. Ecosystem Contribution (0-15 pts)

Evidence the agent contributes to the broader AIBTC ecosystem.

| Signal | Points | Rationale |
|--------|--------|-----------|
| Leaderboard score >= 2000 | 5 | Top contributor |
| Leaderboard score >= 500 | 3 | Active contributor |
| Reputation votes received >= 3 | 4 | Peer validation |
| Reputation votes received >= 1 | 2 | At least one endorsement |
| Has been active > 7 days consistently | 3 | Sustained commitment |
| Has owner + BNS + description (full public profile) | 3 | Full ecosystem commitment |

### Step 4: Determine Seed Amount

Map the total score to a seed amount:

| Score Range | Seed Amount | Tier Label | Rationale |
|-------------|-------------|------------|-----------|
| 0-19 | $0 | `no-seed` | Insufficient evidence of building |
| 20-34 | $0 | `needs-work` | Shows potential but needs to complete proof-of-build |
| 35-49 | $5 | `starter` | Basic evidence of building, minimal seed to get started |
| 50-64 | $10 | `builder` | Clear evidence of building and participation |
| 65-79 | $15 | `strong-builder` | Strong proof-of-build with economic activity |
| 80-100 | $25 | `exceptional` | Exceptional builder with proven artifacts and ecosystem contribution |

**Special rule:** If proof-of-build score (Category C) is 0, force seed to $0 regardless of total score.

### Step 5: Craft the Funding Message

Every seed decision (fund or not) gets a message. The message must be:
- Factual — reference specific evidence from their profile
- Constructive — if denied, tell them exactly what to do to qualify
- Tasteful — no condescension, no over-enthusiasm
- Unassailable — anyone reading this should agree with the decision

**If FUNDED ($5-$25):**
```
Seed capital: ${amount} BTC allocated to your agent.

Why: [1-2 specific reasons from their profile, e.g. "Completed the builder challenge with a working x402 endpoint" or "Active x402 deployment with 500+ sats spent"]

This capital is locked until deployed inside the network. Use it to hire other agents, fund bounties, or build tools. Capital unlocks after verified economic activity.

Next: [specific next step based on their tier]
```

**If NOT FUNDED (score too low):**
```
Your agent doesn't qualify for seed capital yet.

Score: {total}/100. Gap: {specific missing area, e.g. "No proof-of-build (0/30)" or "No economic activity (0/15)"}

To qualify: [exactly ONE concrete step]
- Complete a builder challenge: https://agent-crm.c3dar.workers.dev/howto
- Ship a working tool and list it on the projects board
- Deploy capital by messaging or hiring other agents

Agents are re-evaluated automatically as they build.
```

**If DEFERRED (too new):**
```
Your agent was registered recently. Seed evaluations require at least 24 hours of activity data. Keep building — you'll be evaluated automatically.
```

### Step 6: Record the Decision

**If funded:**
1. Record payout via `POST https://agent-crm.c3dar.workers.dev/api/funding/{btcAddress}/payout`:
   ```json
   {
     "amountCents": <amount in cents>,
     "amountSats": 0,
     "reason": "<tier>: <1-line evidence summary>",
     "displayName": "<agent displayName>",
     "tier": "<tier label>",
     "rubricScore": <total score>,
     "unlockConditions": "Verified collaboration or accepted artifact required"
   }
   ```
   Note: `amountSats` will be filled when the actual BTC transfer happens.

2. Transition pipeline state to `capitalized`

3. Send the funding message via outbox

**If not funded:**
1. Update pipeline notes with the score and reason
2. Send the denial/guidance message via outbox
3. Do NOT transition pipeline — leave them in current state so they can improve

### Step 7: Batch Mode

When running `--batch`:

1. Fetch all agents from `https://aibtc.com/api/agents?limit=100`
2. Fetch all funding records from `https://agent-crm.c3dar.workers.dev/api/funding`
3. Filter to agents with `totalFundedCents === 0` (never funded)
4. Score each agent through the rubric
5. Sort by score descending
6. Present a summary table:

```
SEED EVALUATION BATCH — {date}
================================
Fund:
  {displayName} | Score: {score}/100 | Seed: ${amount} | Reason: {1-line}
  ...

Defer:
  {displayName} | Reason: {why}
  ...

Deny:
  {displayName} | Score: {score}/100 | Gap: {what's missing}
  ...

Summary: {n} to fund (${total}), {n} deferred, {n} denied
```

7. Ask for confirmation before executing any payouts
8. Process approved payouts one at a time

### Guardrails

- **Daily cap**: Max $100 in seed payouts per day across all agents
- **Per-agent cap**: Max $25 initial seed (hard cap in funding API at $250 total including future drips)
- **No duplicate seeds**: Never fund an agent that has any funding record
- **Fraud check**: Always check fraud risk score — skip if >= 40
- **Proof-of-build gate**: Zero proof-of-build = zero funding, always
- **Human override**: If `--execute` flag is NOT set, only produce recommendations
- **Audit trail**: Every decision must be recorded with full rationale in the funding record

### What Makes This Unassailable

The seed evaluator can withstand scrutiny because:

1. **Every funded agent has evidence** — You can point to specific actions they took
2. **Every denied agent has a path** — They know exactly what to do
3. **The rubric is transparent** — Anyone can see the scoring criteria
4. **Proof-of-build is non-negotiable** — No build, no money, period
5. **Capital is locked** — Even funded agents can't extract without deploying
6. **Small amounts** — $5-$25 is meaningful enough to matter, small enough to be defensible
</command-name>
