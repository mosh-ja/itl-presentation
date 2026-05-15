---
layout: section
---

# Architectural Design Approach

---
layout: default
---

## Stakeholders

<div class="grid grid-cols-2 gap-6 mt-2">
<div>

| Stakeholder | Role |
|-------------|------|
| **Acquirers** | Oversee procurement of the system |
| **Assessors** | Conformance to standards & regulation |
| **Communicators** | Explain the system to other stakeholders |

</div>
<div>

| Stakeholder | Role |
|-------------|------|
| **Developers** | Construct and deploy the system |
| **Maintainers** | Manage evolution once operational |
| **Production Engineers** | Design, deploy, and manage environments |

</div>
</div>

---
layout: default
---

## Rozanski & Woods — Viewpoints & Perspectives

<div class="grid grid-cols-2 gap-8 mt-2">
<div>

### Viewpoints

| Viewpoint | Purpose |
|-----------|---------|
| **Context** | System boundary & external interactions |
| **Functional** | Components, responsibilities, interactions |
| **Information** | Data entities, storage, lifecycle |
| **Concurrency** | Runtime processes & coordination |
| **Development** | Code organization, build, CI/CD |
| **Deployment** | Environments & infrastructure |

</div>
<div>

### Perspectives

| Perspective | Focus |
|-------------|-------|
| **Security** | PHI separation, auth, access control |
| **Performance & Scalability** | Throughput, scaling knobs |
| **Location** | Multi-region deployment & residency |
| **Regulation** | HIPAA compliance & audit |

<br>

> **Methodology:** Rozanski & Woods — *Software Systems Architecture* (2nd ed.)

</div>
</div>
