---
theme: default
title: "Datalake Pipeline Platform — Architecture Description"
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
layout: cover
mermaid:
  theme: base
  themeVariables:
    background: '#0B0F14'
    primaryColor: '#1e2a38'
    primaryBorderColor: '#3a5068'
    primaryTextColor: '#dce6f0'
    lineColor: '#7ab8e0'
    edgeColor: '#7ab8e0'
---

<div style="display:flex; height:100%; align-items:center;">

<div style="flex:1; display:flex; flex-direction:column; justify-content:center; align-items:flex-start; padding-left:2rem;">

<h1 style="font-size:3rem; font-weight:700; color:#ffffff; margin:0 0 0.5rem;">
  Viz Data Pipeline
</h1>

<h2 style="font-size:1.6rem; font-weight:400; color:#f47721; border:none; margin:0 0 2rem; padding:0;">
  Architecture Overview
</h2>

<p style="color:#aaaaaa; font-size:1rem; margin:0;">
  Moshe Jacobson · May 2026
</p>

</div>

<div style="flex:1; display:flex; align-items:center; justify-content:center; height:100%;">
  <div style="width:80%; aspect-ratio:4/3; border:2px dashed #555; border-radius:8px; display:flex; align-items:center; justify-content:center; color:#666; font-size:1rem;">
    Cover image
  </div>
</div>

</div>

---
layout: default
---

## Agenda

<div style="margin-top:1.5rem; display:flex; flex-direction:column; gap:1.2rem;">

<div>
  <p style="font-size:1.4rem; font-weight:600; color:#f47721; margin:0 0 0.4rem;">Introduction</p>
  <ul style="margin:0; padding-left:1.4rem;">
    <li>About Me</li>
    <li>Requirements at a Glance</li>
    <li>Stakeholders</li>
  </ul>
</div>

<div>
  <p style="font-size:1.4rem; font-weight:600; color:#f47721; margin:0 0 0.4rem;">Viewpoints and Perspectives</p>
  <ul style="margin:0; padding-left:1.4rem;">
    <li>Existing system</li>
    <li>Shifting to Dagster</li>
  </ul>
</div>

<div>
  <p style="font-size:1.4rem; font-weight:600; color:#f47721; margin:0 0 0.4rem;">Wrap-up</p>
  <ul style="margin:0; padding-left:1.4rem;">
    <li>Tech Stack & Key Decisions</li>
  </ul>
</div>

</div>

---
src: ./pages/01-intro.md
---
---
src: ./pages/02-context.md
---
---
src: ./pages/03-functional.md
---
---
src: ./pages/04-information.md
---
---
src: ./pages/05-concurrency.md
---
---
src: ./pages/06-development.md
---
---
src: ./pages/07-deployment.md
---
---
src: ./pages/10-perspectives.md
---
---
src: ./pages/11-summary.md
---
