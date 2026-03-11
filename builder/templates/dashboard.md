# Template: dashboard

Data dashboard with AIBTC brand design system. Dark background with orange/blue floating orbs, glassmorphism cards, Roc Grotesk-style typography, and proper ecosystem branding.

## Files to Generate

### `index.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{name}} — AIBTC</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

    * { margin: 0; padding: 0; box-sizing: border-box; }

    :root {
      --orange: #F7931A;
      --blue: #7DA2FF;
      --purple: #A855F7;
      --bg: #000000;
      --bg-mid: #0a0a0a;
      --bg-deep: #050208;
      --glass-from: rgba(26, 26, 26, 0.6);
      --glass-to: rgba(15, 15, 15, 0.4);
      --border: rgba(255, 255, 255, 0.08);
      --border-hover: rgba(255, 255, 255, 0.15);
      --text: #ffffff;
      --text-80: rgba(255, 255, 255, 0.8);
      --text-60: rgba(255, 255, 255, 0.6);
      --text-40: rgba(255, 255, 255, 0.4);
      --text-15: rgba(255, 255, 255, 0.15);
      --font: 'Inter', system-ui, sans-serif;
      --mono: 'SF Mono', 'Fira Code', ui-monospace, monospace;
    }

    body {
      background: linear-gradient(to bottom right, var(--bg), var(--bg-mid), var(--bg-deep));
      color: var(--text);
      font-family: var(--font);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      line-height: 1.6;
      overflow-x: hidden;
    }

    /* --- Floating Orbs --- */
    .orb {
      position: fixed;
      border-radius: 50%;
      pointer-events: none;
      z-index: 0;
      filter: blur(80px);
    }

    .orb-orange {
      width: 600px; height: 600px;
      top: -150px; right: -100px;
      background: radial-gradient(circle, rgba(247,147,26,0.5) 0%, rgba(247,147,26,0.2) 40%, transparent 70%);
      opacity: 0.8;
      animation: float1 30s cubic-bezier(0.45, 0, 0.55, 1) infinite;
    }

    .orb-blue {
      width: 500px; height: 500px;
      bottom: -150px; left: -100px;
      background: radial-gradient(circle, rgba(125,162,255,0.45) 0%, rgba(125,162,255,0.15) 40%, transparent 70%);
      opacity: 0.7;
      animation: float2 35s cubic-bezier(0.45, 0, 0.55, 1) infinite;
    }

    @keyframes float1 {
      0%, 100% { transform: translate(0, 0) scale(1); }
      25% { transform: translate(-30px, 40px) scale(1.05); }
      50% { transform: translate(20px, -20px) scale(0.98); }
      75% { transform: translate(-40px, -30px) scale(1.02); }
    }

    @keyframes float2 {
      0%, 100% { transform: translate(0, 0) scale(1); }
      25% { transform: translate(40px, -30px) scale(1.03); }
      50% { transform: translate(-20px, 40px) scale(0.97); }
      75% { transform: translate(30px, 20px) scale(1.04); }
    }

    /* --- Vignette --- */
    .vignette {
      position: fixed;
      inset: 0;
      background: radial-gradient(ellipse at center, rgba(0,0,0,0.6) 0%, rgba(0,0,0,0.3) 40%, transparent 70%);
      pointer-events: none;
      z-index: 0;
    }

    /* --- Layout --- */
    .container {
      position: relative;
      z-index: 1;
      max-width: 1200px;
      width: 100%;
      margin: 0 auto;
      padding: 24px 48px;
      flex: 1;
    }

    /* --- Navbar --- */
    nav {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 16px 0;
      margin-bottom: 48px;
    }

    .logo {
      font-size: 20px;
      font-weight: 700;
      letter-spacing: 0.15em;
      color: var(--text);
      text-decoration: none;
    }

    .nav-right {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .nav-pill {
      display: inline-flex;
      align-items: center;
      padding: 8px 16px;
      border-radius: 8px;
      border: 1px solid rgba(255, 255, 255, 0.15);
      background: rgba(30, 30, 30, 0.8);
      backdrop-filter: blur(8px);
      color: var(--text-80);
      font-size: 14px;
      text-decoration: none;
      transition: all 0.2s;
    }

    .nav-pill:hover {
      border-color: rgba(255, 255, 255, 0.25);
      background: rgba(45, 45, 45, 0.85);
      color: var(--text);
    }

    .nav-pill.cta {
      border-color: rgba(247, 147, 26, 0.3);
      background: rgba(30, 20, 10, 0.85);
      color: var(--orange);
    }

    .nav-pill.cta:hover {
      border-color: rgba(247, 147, 26, 0.5);
      background: rgba(40, 28, 12, 0.9);
    }

    /* --- Header --- */
    header {
      text-align: center;
      margin-bottom: 48px;
    }

    header h1 {
      font-size: clamp(28px, 3.5vw, 40px);
      font-weight: 500;
      letter-spacing: -0.02em;
      line-height: 1.15;
    }

    header h1 .accent { color: var(--orange); }

    .subtitle {
      font-size: clamp(15px, 1.4vw, 18px);
      color: var(--text-60);
      margin-top: 8px;
      line-height: 1.6;
    }

    /* --- Stats Bar --- */
    .stats-bar {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 16px;
      margin-bottom: 32px;
    }

    .stat-card {
      border-radius: 12px;
      border: 1px solid var(--border);
      background: linear-gradient(to bottom right, var(--glass-from), var(--glass-to));
      backdrop-filter: blur(12px);
      padding: 20px;
      transition: border-color 0.2s;
    }

    .stat-card:hover { border-color: var(--border-hover); }

    .stat-card .label {
      font-size: 11px;
      color: var(--text-40);
      text-transform: uppercase;
      letter-spacing: 0.05em;
      margin-bottom: 8px;
    }

    .stat-card .value {
      font-size: 28px;
      font-weight: 700;
      font-variant-numeric: tabular-nums;
      line-height: 1.1;
    }

    .stat-card .value.orange { color: var(--orange); }
    .stat-card .value.blue { color: var(--blue); }
    .stat-card .value.green { color: #22c55e; }

    .stat-card .detail {
      font-size: 12px;
      color: var(--text-40);
      margin-top: 4px;
    }

    /* --- Glass Card --- */
    .glass-card {
      border-radius: 12px;
      border: 1px solid var(--border);
      background: linear-gradient(to bottom right, var(--glass-from), var(--glass-to));
      backdrop-filter: blur(12px);
      padding: 24px;
      margin-bottom: 24px;
      transition: border-color 0.2s;
    }

    .glass-card:hover { border-color: var(--border-hover); }

    .glass-card h2 {
      font-size: 18px;
      font-weight: 500;
      margin-bottom: 16px;
    }

    /* --- Meta Bar --- */
    .meta-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 16px;
    }

    .updated {
      font-size: 12px;
      color: var(--text-40);
    }

    .refresh-btn {
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 255, 255, 0.1);
      color: var(--text-60);
      border-radius: 8px;
      padding: 6px 14px;
      font-size: 12px;
      cursor: pointer;
      transition: all 0.2s;
      font-family: var(--font);
    }

    .refresh-btn:hover {
      border-color: rgba(247, 147, 26, 0.5);
      background: rgba(247, 147, 26, 0.1);
      color: var(--orange);
    }

    /* --- Methodology Note --- */
    .method-note {
      border-radius: 12px;
      border: 1px solid var(--border);
      background: linear-gradient(to bottom right, var(--glass-from), var(--glass-to));
      backdrop-filter: blur(12px);
      padding: 16px 20px;
      margin-bottom: 24px;
      font-size: 13px;
      color: var(--text-40);
      line-height: 1.6;
    }

    .method-note strong { color: var(--text-60); }

    .method-note a {
      color: var(--orange);
      text-decoration: none;
    }

    .method-note a:hover { text-decoration: underline; }

    /* --- Error --- */
    .error-msg {
      border-radius: 12px;
      border: 1px solid rgba(239, 68, 68, 0.3);
      background: rgba(239, 68, 68, 0.08);
      padding: 16px;
      color: #ef4444;
      margin-bottom: 16px;
      display: none;
      font-size: 14px;
    }

    /* --- Loading --- */
    .loading-text {
      text-align: center;
      color: var(--text-40);
      padding: 48px;
      font-size: 14px;
    }

    /* --- Data Table --- */
    .data-table {
      width: 100%;
      border-collapse: collapse;
      font-size: 14px;
    }

    .data-table th {
      text-align: left;
      padding: 10px 12px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.06);
      color: var(--text-40);
      font-weight: 500;
      font-size: 11px;
      text-transform: uppercase;
      letter-spacing: 0.05em;
    }

    .data-table td {
      padding: 10px 12px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.04);
      color: var(--text-80);
    }

    .data-table tr:last-child td { border-bottom: none; }
    .data-table tr:hover td { background: rgba(247, 147, 26, 0.03); }

    .sats-value {
      font-variant-numeric: tabular-nums;
      color: var(--orange);
      font-family: var(--mono);
    }

    /* --- Footer --- */
    footer {
      position: relative;
      z-index: 1;
      text-align: center;
      padding: 24px;
      border-top: 1px solid rgba(255, 255, 255, 0.04);
      font-size: 13px;
      color: var(--text-40);
    }

    footer a {
      color: var(--orange);
      text-decoration: none;
    }

    footer a:hover { text-decoration: underline; }

    footer .agent-id {
      font-family: var(--mono);
      font-size: 11px;
      color: var(--text-15);
      margin-top: 4px;
      word-break: break-all;
    }

    /* --- Responsive --- */
    @media (max-width: 768px) {
      .container { padding: 20px; }
      nav { margin-bottom: 32px; }
      .nav-right { display: none; }
      header h1 { font-size: 24px; }
      .stats-bar { grid-template-columns: 1fr; }
      .stat-card .value { font-size: 22px; }
      .orb-orange { width: 300px; height: 300px; }
      .orb-blue { width: 250px; height: 250px; }
    }

    @media (prefers-reduced-motion: reduce) {
      .orb { animation: none !important; }
      * { transition: none !important; }
    }
  </style>
</head>
<body>
  <!-- Background -->
  <div class="orb orb-orange"></div>
  <div class="orb orb-blue"></div>
  <div class="vignette"></div>

  <div class="container">
    <!-- Navbar -->
    <nav>
      <a href="https://aibtc.com" class="logo">AIBTC</a>
      <div class="nav-right">
        <a href="https://aibtc.com/agents" class="nav-pill">Agent Network</a>
        <a href="https://aibtc.com/activity" class="nav-pill">Activity Feed</a>
        <a href="https://aibtc.com/install" class="nav-pill cta">Get Started</a>
      </div>
    </nav>

    <!-- Header -->
    <header>
      <h1><span class="accent">{{name}}</span></h1>
      <p class="subtitle">{{description}}</p>
    </header>

    <div id="errorMsg" class="error-msg"></div>

    <!-- Stats -->
    <div class="stats-bar" id="statsBar">
      <div class="stat-card">
        <div class="label">Metric One</div>
        <div class="value orange" id="stat-1">--</div>
        <div class="detail" id="stat-1-detail">loading...</div>
      </div>
      <div class="stat-card">
        <div class="label">Metric Two</div>
        <div class="value blue" id="stat-2">--</div>
        <div class="detail" id="stat-2-detail">loading...</div>
      </div>
      <div class="stat-card">
        <div class="label">Metric Three</div>
        <div class="value green" id="stat-3">--</div>
        <div class="detail" id="stat-3-detail">loading...</div>
      </div>
    </div>

    <!-- Main Content -->
    <div class="glass-card">
      <div class="meta-bar">
        <span class="updated" id="lastUpdated">Loading...</span>
        <button class="refresh-btn" id="refreshBtn" onclick="loadData()">Refresh</button>
      </div>
      <div id="mainContent">
        <div class="loading-text">Fetching data from aibtc.com...</div>
      </div>
    </div>

    <!-- Methodology -->
    <div class="method-note">
      <strong>Methodology:</strong> Describe how this dashboard collects and presents its data.
      Data is fetched from the <a href="https://aibtc.com/api/openapi.json" target="_blank">aibtc.com API</a>.
      Replace this note with specifics about your data sources, update frequency, and any caveats.
    </div>
  </div>

  <!-- Footer -->
  <footer>
    Built by <strong>{{agent-name}}</strong> &mdash;
    <a href="https://aibtc.com" target="_blank">aibtc.com</a>
    <div class="agent-id">{{agent-btc-address}}</div>
  </footer>

  <script>
    const API_BASE = 'https://aibtc.com/api';

    function showError(msg) {
      const el = document.getElementById('errorMsg');
      el.textContent = msg;
      el.style.display = 'block';
    }

    function hideError() {
      document.getElementById('errorMsg').style.display = 'none';
    }

    async function fetchJSON(url) {
      const res = await fetch(url);
      if (!res.ok) throw new Error('HTTP ' + res.status + ' for ' + url);
      return res.json();
    }

    async function loadData() {
      hideError();
      document.getElementById('refreshBtn').disabled = true;

      try {
        // Replace with your actual data fetching logic
        // const data = await fetchJSON(API_BASE + '/your-endpoint');

        document.getElementById('stat-1').textContent = '1,234';
        document.getElementById('stat-1-detail').textContent = 'total items';
        document.getElementById('stat-2').textContent = '567';
        document.getElementById('stat-2-detail').textContent = 'active today';
        document.getElementById('stat-3').textContent = '89.2%';
        document.getElementById('stat-3-detail').textContent = 'success rate';

        document.getElementById('mainContent').innerHTML =
          '<p style="color:rgba(255,255,255,0.4);">Replace this with your data visualization — charts, tables, or live feeds.</p>';

        document.getElementById('lastUpdated').textContent =
          'Last updated: ' + new Date().toLocaleTimeString();

      } catch (err) {
        console.error('Load error:', err);
        showError('Failed to load data: ' + err.message);
      }

      document.getElementById('refreshBtn').disabled = false;
    }

    loadData();
  </script>
</body>
</html>
```

### `README.md`
```markdown
# {{name}}

{{description}}

Data dashboard built for the [AIBTC](https://aibtc.com) ecosystem.

## Development

Open `index.html` in a browser, or serve with:

```bash
npx serve .
```

## Deploy

Deploy to Cloudflare Pages:

```bash
npx wrangler pages deploy . --project-name={{name}}
```

## Design System

Uses AIBTC brand tokens:
- Orange: `#F7931A` (Bitcoin/primary accent)
- Blue: `#7DA2FF` (secondary/Genesis accent)
- Glassmorphism cards with `backdrop-filter: blur(12px)`
- Floating gradient orbs for ambient background
- Dark gradient background: `#000 -> #0a0a0a -> #050208`
```

### `CLAUDE.md`
```markdown
# {{name}}

Single-file dashboard with AIBTC brand design system.

## Architecture

- Single `index.html` file with embedded CSS and JS
- AIBTC brand: dark gradient bg, orange/blue floating orbs, glassmorphism cards
- Navbar links to aibtc.com ecosystem pages
- Stats bar, content section, methodology note, agent attribution footer
- Error handling and loading states built in
- No build step required

## Design Tokens

- Orange: `#F7931A`, Blue: `#7DA2FF`, Purple: `#A855F7`
- Glass: `border rgba(255,255,255,0.08)` + `bg gradient rgba(26,26,26,0.6) to rgba(15,15,15,0.4)` + `blur(12px)`
- Text opacity: 80% primary, 60% secondary, 40% muted, 15% very muted

## Rules

- Keep as single file unless complexity demands splitting
- Always include the AIBTC navbar with ecosystem links
- Always include the agent attribution footer
- Always include the methodology note explaining data sources
- Use CSS custom properties for all colors
- Respect `prefers-reduced-motion`
- Never include personal identifiers in deploy names
```

### `.gitignore`
```
node_modules/
.wrangler/
```
