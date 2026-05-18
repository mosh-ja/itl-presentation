# Datalake Pipeline Platform — Architecture Presentation

Slidev presentation for the Viz Data Pipeline architecture description, following the Rozanski & Woods viewpoints and perspectives methodology.

## Quick Start

```bash
npm install
npm run dev      # http://localhost:3030 — live reload
npm run build    # outputs to dist/
npm run export   # export to PDF/PPTX
```

## Structure

```
slides.md               # Entry point — global config + page imports
style.css               # Global brand styles (Tikal: #111111 bg, #f47721 orange)
public/
  logo-white.png        # Tikal logo (white, for dark backgrounds)
  logo.png              # Tikal logo (dark, for light contexts)
pages/
  01-cover.md           # Cover + Agenda
  02-intro.md           # About Me + Requirements at a Glance
  03-approach.md        # Stakeholders + R&W methodology
  04-context.md         # Context Viewpoint
  05-functional.md      # Functional Viewpoint
  06-information.md     # Information Viewpoint
  07-concurrency.md     # Concurrency Viewpoint
  08-development.md     # Development Viewpoint
  09-deployment.md      # Deployment Viewpoint
  10-perspectives.md    # Security · Performance · Location · Regulation
  11-summary.md         # Tech Stack + Thank You
```

## Source Material

| File | Role |
|------|------|
| `../AD.md` | Primary architecture documentation |
| `../.specify/architect/views/platform/` | Per-viewpoint detail files |
| `template.pdf` / `template.pptx` | Visual template reference (Tikal) |
| `implementation-guide.md` | Slidev techniques and patterns used in this project |
