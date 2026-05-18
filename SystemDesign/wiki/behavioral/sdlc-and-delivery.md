---
title: SDLC and Agile Delivery
type: behavioral
tags: [sdlc, agile, scrum, kanban, delivery, behavioral]
sources: [leadership]
created: 2026-05-16
updated: 2026-05-16
---

# SDLC and Agile Delivery

SDLC phases, model comparison, and Agile methodology Q&A for EM/SA interviews
(see [[sources/leadership-study-guide]]).

## SDLC Phases

| Phase | Key Activities | Output |
|---|---|---|
| **Requirements** | Stakeholder meetings; BRD/PRD; feasibility assessment | Validated requirements |
| **Design** | HLD (architecture, tech stack) + LLD (modules, APIs, DB schema); security & scalability | Architecture & design docs |
| **Development** | Coding to spec; peer reviews; unit tests; CI setup | Working software components |
| **Testing** | Unit → Integration → System → Performance → Security → UAT | Quality-assured product |
| **Deployment** | CI/CD pipeline; Blue-Green or Canary deploy; rollback strategy | Software available to users |
| **Maintenance** | Bug fixes; performance monitoring; enhancements; incident management; tech debt | Stable and evolving system |

## SDLC Models

| Model | Key Trait | Best For |
|---|---|---|
| **Waterfall** | Sequential — each phase must complete before next begins; changes are costly mid-flight | Government, regulated industries, well-defined scope |
| **Agile — Scrum** | Fixed sprints (2–4 weeks); Product Owner + Scrum Master + Dev Team; ceremonies (Planning, Standup, Review, Retro) | Fast-changing product environments |
| **Agile — Kanban** | Continuous flow (no sprints); visual board; WIP limits reduce bottlenecks | Support teams, continuous delivery |
| **Spiral** | Risk-driven iterative; each cycle = plan → risk analysis → engineering → evaluate | Large, complex, high-risk systems |
| **V-Model** | Testing phase mapped to each dev phase (Requirements↔UAT, Design↔System Test, Impl↔Unit Test); early test planning | Strict quality control |
| **DevOps** | Dev + Ops integration; CI/CD automation; continuous testing + deployment; rapid feedback | Cloud-native, fast-release products |

**One-liners:**
- Waterfall → sequential and structured.
- Agile (Scrum/Kanban) → iterative and flexible.
- Spiral → risk-driven iterative.
- V-Model → testing-focused waterfall extension.
- DevOps → automation + continuous delivery model.

## Agile Methodology

### Core Principles (Agile Manifesto, 2001)
- Customer satisfaction through early and continuous delivery.
- Welcome change even late in development.
- Frequent delivery of working software (weeks, not months).
- Collaboration between business and developers.
- Sustainable pace; technical excellence; simplicity; self-organizing teams.
- Regular reflection and adaptation.

### Key Agile Terms

| Term | Meaning | Why It Matters |
|---|---|---|
| **Sprint** | Time-boxed iteration (1–4 weeks) | Core to Scrum; defines delivery cadence |
| **Backlog** | Ordered list of work items | Ensures prioritization and transparency |
| **User Story** | Short description of functionality | Keeps focus on customer value |
| **Epic** | Large body of work broken into stories | Manages scope at higher level |
| **Scrum Master** | Facilitates ceremonies; removes impediments; coaches Agile | Process & team health |
| **Product Owner** | Owns backlog; defines priorities; represents customer | Product & business alignment |
| **Daily Standup** | 15-min team sync | Transparency and quick issue resolution |
| **Velocity** | Work completed per sprint | Forecasts future capacity |
| **Burndown Chart** | Remaining work vs. time | Tracks progress and predicts completion |
| **Definition of Done** | Agreed criteria for completed work | Ensures quality and consistency |
| **Retrospective** | Post-sprint meeting to reflect and improve | Drives continuous improvement |
| **WIP Limit** | Cap on in-flight Kanban items | Reduces bottlenecks; improves throughput |
| **Acceptance Criteria** | Conditions for user story completion | Guides testing and validation |

### Interview Q&A

**Q: How do you manage backlog prioritization in Agile?**
Techniques:
- **MoSCoW** (Must/Should/Could/Won't) — classify by criticality.
- **WSJF** (Weighted Shortest Job First) — prioritize by ROI vs. effort.
- **Business Value Scoring** — rank by client impact.

"I'd recommend backlog refinement sessions with stakeholders, using WSJF to ensure
high-value features are delivered first while balancing technical debt."

**Q: What's the role of a Scrum Master vs. Product Owner?**

| Role | Focus | Key Responsibilities |
|---|---|---|
| **Scrum Master** | Process & team | Facilitates ceremonies, removes impediments, coaches Agile practices |
| **Product Owner** | Product & business | Owns backlog, defines priorities, represents customer needs |

"The Scrum Master ensures the team follows Agile principles, while the Product Owner
ensures the work aligns with business goals. Together, they balance delivery discipline
with customer value."

**Q: How do you measure success in Agile projects?**
- **Quantitative:** Velocity & throughput; lead time & cycle time; defect rate.
- **Qualitative:** Customer satisfaction (NPS); team morale; sustainability.

"Success isn't just velocity — it's delivering business value predictably, with high
quality and satisfied stakeholders. I'd measure both delivery metrics and customer
outcomes."

### Common Agile Challenges
- **Scope creep** — Agile welcomes change, but without discipline, projects drift.
- **Team maturity** — requires self-organizing teams; weak collaboration undermines it.
- **Client buy-in** — stakeholders must accept iterative delivery and evolving requirements.

## Related
[[behavioral/em-technical-leadership]] · [[behavioral/em-people-management]] ·
[[patterns/deployment-strategies]] · [[scenarios/production-incident-response]]
