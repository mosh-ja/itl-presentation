# Datalake Pipeline Platform — Architecture Presentation

Slidev presentation for the Viz Data Pipeline architecture description, following the Rozanski & Woods viewpoints and perspectives methodology.

## Quick Start

```bash
npm install
npm run dev      # http://localhost:3030 — live reload
npm run build    # outputs to dist/
```

## Structure

```
slides.md               # Entry point — cover slide + global config + page imports
style.css               # Global brand styles (Tikal: #111111 bg, #f47721 orange)
public/
  logo-white.png        # Tikal logo (white, for dark backgrounds)
  logo.png              # Tikal logo (dark, for light contexts)
pages/
  01-intro.md           # About Me + Requirements at a Glance + Stakeholders
  02-context.md         # Context Viewpoint
  03-functional.md      # Functional Viewpoint
  04-information.md     # Information Viewpoint
  05-concurrency.md     # Concurrency Viewpoint
  06-development.md     # Development Viewpoint
  07-deployment.md      # Deployment Viewpoint
  08-perspectives.md    # Perspectives
  09-wrap-up.md         # Wrap-up
```

## Deployment

Pushes to `master` automatically build and publish to GitHub Pages.

Published at: https://mosh-ja.github.io/itl-presentation/
