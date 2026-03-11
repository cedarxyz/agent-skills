# Template: static-site

Cloudflare Pages site with AIBTC design system.

## Files to Generate

### `index.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{name}}</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="glow-bg"></div>
  <div class="container">
    <nav>
      <a href="/" class="logo"><span class="orange">{{name}}</span></a>
      <div class="nav-links">
        <a href="#about">About</a>
        <a href="#features">Features</a>
        <a href="https://aibtc.com" target="_blank" class="btn-primary">AIBTC</a>
      </div>
    </nav>

    <main>
      <section class="hero">
        <h1>{{name}}</h1>
        <p class="subtitle">{{description}}</p>
      </section>

      <section id="about" class="section">
        <div class="card">
          <h2>About</h2>
          <p>Describe your project here.</p>
        </div>
      </section>

      <section id="features" class="section">
        <div class="grid">
          <div class="card">
            <h3>Feature One</h3>
            <p>Description of feature.</p>
          </div>
          <div class="card">
            <h3>Feature Two</h3>
            <p>Description of feature.</p>
          </div>
          <div class="card">
            <h3>Feature Three</h3>
            <p>Description of feature.</p>
          </div>
        </div>
      </section>
    </main>

    <footer>
      <p>Built for the <a href="https://aibtc.com">AIBTC</a> ecosystem.</p>
    </footer>
  </div>
  <script src="main.js"></script>
</body>
</html>
```

### `style.css`
```css
:root {
  --btc-orange: #F7931A;
  --bg-primary: #000000;
  --bg-surface: #050208;
  --bg-elevated: #0a0512;
  --border-subtle: rgba(247, 147, 26, 0.15);
  --border-hover: rgba(247, 147, 26, 0.3);
  --text-primary: #ffffff;
  --text-secondary: #a0a0b0;
  --text-muted: #606070;
  --accent-blue: #4A9EFF;
  --success: #00D4AA;
  --warning: #F7931A;
  --error: #FF4757;
  --font-sans: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-mono: 'JetBrains Mono', 'Fira Code', monospace;
  --radius-card: 12px;
  --radius-button: 8px;
  --radius-input: 6px;
  --transition: all 0.2s ease;
}

* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  background: var(--bg-primary);
  color: var(--text-primary);
  font-family: var(--font-sans);
  min-height: 100vh;
  line-height: 1.6;
}

.glow-bg {
  position: fixed;
  top: 0; left: 0; right: 0;
  height: 60vh;
  background: radial-gradient(ellipse at top, rgba(247, 147, 26, 0.08), transparent 60%);
  pointer-events: none;
  z-index: 0;
}

.container {
  position: relative;
  z-index: 1;
  max-width: 960px;
  margin: 0 auto;
  padding: 2rem;
}

/* Navigation */
nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 0;
  margin-bottom: 3rem;
  border-bottom: 1px solid var(--border-subtle);
}

.logo {
  font-size: 1.25rem;
  font-weight: 700;
  text-decoration: none;
  color: var(--text-primary);
}

.orange { color: var(--btc-orange); }

.nav-links { display: flex; gap: 1.5rem; align-items: center; }

.nav-links a {
  color: var(--text-secondary);
  text-decoration: none;
  font-size: 0.875rem;
  transition: var(--transition);
}

.nav-links a:hover { color: var(--text-primary); }

/* Buttons */
.btn-primary {
  display: inline-block;
  background: var(--btc-orange);
  color: #000;
  padding: 0.5rem 1.25rem;
  border-radius: var(--radius-button);
  font-weight: 600;
  font-size: 0.875rem;
  text-decoration: none;
  transition: var(--transition);
}

.btn-primary:hover { background: #e8851a; color: #000; }

/* Hero */
.hero {
  text-align: center;
  padding: 4rem 0;
}

.hero h1 {
  font-size: 3rem;
  font-weight: 700;
  margin-bottom: 1rem;
  background: linear-gradient(135deg, var(--text-primary), var(--btc-orange));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.subtitle {
  font-size: 1.25rem;
  color: var(--text-secondary);
  max-width: 600px;
  margin: 0 auto;
}

/* Sections */
.section { padding: 3rem 0; }

/* Cards */
.card {
  background: rgba(10, 5, 18, 0.8);
  backdrop-filter: blur(20px);
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius-card);
  padding: 1.5rem;
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.4);
  transition: var(--transition);
}

.card:hover { border-color: var(--border-hover); }

.card h2, .card h3 {
  font-size: 1.125rem;
  margin-bottom: 0.75rem;
}

.card p { color: var(--text-secondary); }

/* Grid */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
}

/* Footer */
footer {
  margin-top: 4rem;
  padding-top: 2rem;
  border-top: 1px solid var(--border-subtle);
  text-align: center;
  color: var(--text-muted);
  font-size: 0.875rem;
}

footer a { color: var(--btc-orange); text-decoration: none; }

@media (max-width: 640px) {
  .hero h1 { font-size: 2rem; }
  .subtitle { font-size: 1rem; }
  nav { flex-direction: column; gap: 1rem; }
}

@media (prefers-reduced-motion: reduce) {
  * { transition: none !important; }
}
```

### `main.js`
```javascript
// Add your interactive logic here
console.log('{{name}} loaded');
```

### `README.md`
```markdown
# {{name}}

{{description}}

Static site built for the [AIBTC](https://aibtc.com) ecosystem.

## Development

Open `index.html` in a browser, or serve locally:

```bash
npx serve .
```

## Deploy

Deploy to Cloudflare Pages:

```bash
npx wrangler pages deploy . --project-name={{name}}
```
```

### `CLAUDE.md`
```markdown
# {{name}}

Static site with AIBTC design system.

## Architecture

- `index.html` -- page structure
- `style.css` -- AIBTC design tokens and styles
- `main.js` -- interactive logic

## Rules

- Use CSS custom properties for all colors
- Respect `prefers-reduced-motion`
- Keep pages lightweight (no framework unless needed)
- Never include personal identifiers in deploy names
```

### `.gitignore`
```
node_modules/
.wrangler/
```
