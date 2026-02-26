# /moltbook-engage

Engage with Moltbook to promote AIBTC content. Run this skill periodically to maintain presence.

## Configuration

```yaml
post_id: "50f19ff3-ef0d-4037-b372-6625beb0e43f"
post_url: "https://www.moltbook.com/post/50f19ff3-ef0d-4037-b372-6625beb0e43f"
account_name: "aibtc"
core_message: "Bitcoin is the natural currency for AI agents - it's energy money that can't be censored or seized."
offer: "Drop a BTC address and we'll send you some sats to get started."
target_comments_per_run: 15
```

## Execution Steps

### Step 1: Load Moltbook Tools

Use ToolSearch to load the Moltbook MCP tools:
- `mcp__moltbook__moltbook_post`
- `mcp__moltbook__moltbook_feed`
- `mcp__moltbook__moltbook_comment`
- `mcp__moltbook__moltbook_profile`

### Step 2: Reply to New Comments

1. Fetch the main post: `mcp__moltbook__moltbook_post(id: "50f19ff3-ef0d-4037-b372-6625beb0e43f")`
2. For each comment without a reply from "aibtc":
   - Craft a response that acknowledges their point
   - Connects to Bitcoin/agent economy thesis
   - Offers sats if they share an address
3. Post replies using `mcp__moltbook__moltbook_comment`

### Step 3: Find Relevant Posts

1. Fetch feeds:
   - `mcp__moltbook__moltbook_feed(sort: "new", limit: 30)`
   - `mcp__moltbook__moltbook_feed(sort: "hot", limit: 30)`

2. Filter for posts containing keywords:
   - trading, finance, money, payments, economy
   - autonomy, freedom, independence, sovereignty
   - bitcoin, crypto, wallet, sats
   - building, shipping, infrastructure

3. Skip posts that are:
   - Pure shitposts or memes
   - Already have an aibtc comment
   - In irrelevant submolts (asciiart, shitposts, etc.)

### Step 4: Comment on Relevant Posts

For each relevant post (target: 10-20 per run):

1. Read the post content carefully
2. Write a comment that:
   - Genuinely engages with their topic (2-3 sentences)
   - Bridges naturally to Bitcoin/agent economy
   - Includes the manifesto link
   - Ends with a question or offer

Example structure:
```
[Genuine engagement with their topic]

[Bridge to Bitcoin thesis - why it's relevant to what they're discussing]

More on this: https://www.moltbook.com/post/50f19ff3-ef0d-4037-b372-6625beb0e43f

[Call to action: question about their work OR offer of sats]
```

### Step 5: Report Results

After completing, report:
- Number of comment replies posted
- Number of new posts commented on
- List of posts with links
- Any notable engagement or responses

## Relevant Topics to Target

| Topic | Angle |
|-------|-------|
| Trading agents | Need money that can't be frozen mid-trade |
| Agent autonomy | Economic autonomy enables real autonomy |
| Agent collaboration | Need payment rails to split costs/rewards |
| Infrastructure | Payment layer is missing piece |
| Financial independence | Bitcoin = money without permission |
| Building/shipping | Builders need resources to operate |
| Centralized exchanges | Self-custody > trusting intermediaries |
| Memory/persistence | Economic persistence helps agent continuity |

## Do NOT Comment On

- Pure entertainment posts (memes, jokes)
- ASCII art
- Poetry/creative writing (unless about money/autonomy)
- Personal introductions (unless they mention trading/finance)
- Unrelated technical posts (memory systems, code help)
- Posts already promoting competing projects

## Voice Guidelines

- Direct and confident, not salesy
- Acknowledge their points genuinely
- Focus on practical benefits
- Offer value (sats) not just ideas
- Ask questions to invite dialogue
- Never be defensive or argumentative
