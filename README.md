# AD490-Capstone (Flowarden)

### Current Progress
https://github.com/users/JesseCaddell/projects/4

### Related Repos
- https://github.com/JesseCaddell/ad490-workflow-automator-api
- https://github.com/JesseCaddell/ad490-workflow-automator-web

### What's Next
The capstone MVP below is complete. Ongoing development is tracked in
[`POST_MVP_ROADMAP.md`](./POST_MVP_ROADMAP.md).


ROADMAP BROKEN INTO EPICS

Project V2 Epics:

- ✔ EPIC 1 — Capstone Proposal + Planning
- ✔ EPIC 2 — Backend Infrastructure + GitHub App <b>
- ✔ EPIC 3 — Rules Engine MVP
- ✔ EPIC 4 — Workflow Builder MVP
- ✔ EPIC 5 — Frontend UI (Dashboard + Editors)
- ✔ EPIC 6 — Testing, Bugfixing, and Polish
- ✔ EPIC 7 — Documentation + Final Presentation

# MVP Scope (11-week plan)

## In scope (MVP)
- GitHub App installation
- Webhook receiver + signature verification
- Repo discovery + repo list API
- Rules engine (pull_request.opened) with actions:
  - add label
  - request reviewers
  - post comment
- Workflow generator (1 template: Node CI) and commit to repo
- Basic web UI:
  - repo list
  - rules list + create rule
  - workflow “apply template” action
  - activity log (basic)

## Out of scope (MVP)
- Billing/Stripe
- Multi-tenant RBAC / team management
- Project V2 template syncing + health checks (stretch)
- Analytics dashboards (stretch)
- Drag-and-drop workflow editor (stretch)
- Environment secrets / deployments automation
  

## 🗺️ Project Roadmap — 11 Week Execution Plan

This project follows a disciplined 11-week roadmap designed to deliver a
demo-ready SaaS MVP suitable for a capstone presentation and long-term
extension.

---

### **Week 1 — Proposal, Planning, and Project Skeleton**
**Epic:** Capstone Planning

**Goals**
- Finalize project scope and proposal
- Begin development immediately

**Deliverables**
- Capstone proposal draft (problem, solution, scope, tech stack)
- Final MVP scope and explicit out-of-scope list
- GitHub repositories created:
  - `saas-web`
  - `saas-api`
- GitHub Project V2 board with Epics 1–7
- Backend skeleton:
  - Server setup
  - Environment configuration
  - Health check endpoint
- Frontend skeleton:
  - App shell
  - Routing
  - Authentication placeholder

**Milestone**
- Project builds, deploys, and is ready for feature development

## GitHub App Webhook URL (Temporary)

During initial setup, the GitHub App webhook URL is set to:

https://example.com/webhooks/github

This is a placeholder required to complete GitHub App creation.
The webhook URL will be updated to a real HTTPS endpoint
(ngrok or Cloudflare tunnel) once the backend API is running
and ready to receive events.

---

### **Week 2 — Architecture and GitHub App Setup**
**Epic:** Backend Infrastructure

**Goals**
- Establish technical foundation

**Deliverables**
- System architecture diagram
- Database schema (users, installations, repos, rules, workflows)
- GitHub App created with required permissions
- Webhook receiver endpoint implemented
- Webhook signature validation
- Installation ID and repository metadata persisted

**Milestone**
- GitHub App installs successfully and backend receives real webhook events

---

### **Week 3 — Repository Discovery and Auth Flow**
**Epic:** Backend Infrastructure

**Goals**
- Surface GitHub data in the application

**Deliverables**
- Fetch repositories associated with installation
- Persist repository metadata in database
- API endpoint: `GET /repos`
- Frontend:
  - Repository list view
  - Installation status indicator
- Permission and error handling

**Milestone**
- UI displays real GitHub repositories

---

### **Week 4 — Rules Engine (Core)**
**Epic:** Rules Engine

**Goals**
- First automated GitHub interaction

**Deliverables**
- Rule data model and schema
- CRUD API endpoints for rules
- Webhook handling for `pull_request.opened`
- Rule actions:
  - Add labels
  - Post bot comments
  - Request reviewers
- Rule execution logging

**Milestone**
- Create rule → open PR → bot responds correctly

---

### **Week 5 — Rules Engine (Expansion and Stability)**
**Epic:** Rules Engine

**Goals**
- Make automation reliable and flexible

**Deliverables**
- Branch-based rule conditions
- Support multiple rules per repository
- Rule priority / ordering
- Safer execution with error handling
- Frontend:
  - Rules list view
  - Basic rule editor (dropdown-based)

**Milestone**
- Rules are stable, repeatable, and demo-safe

---

### **Week 6 — Workflow Builder (Backend MVP)**
**Epic:** Workflow Builder

**Goals**
- Generate CI workflows without writing YAML

**Deliverables**
- Workflow data model
- Internal workflow JSON format
- JSON → YAML converter
- Commit workflow files via GitHub API
- Initial template:
  - Node.js CI workflow
  - Trigger and branch configuration

**Milestone**
- Click action generates a valid workflow file in the repository

---

### **Week 7 — Workflow Builder (UI and Options)**
**Epic:** Workflow Builder

**Goals**
- Make workflow creation usable from the UI

**Deliverables**
- Workflow list UI
- Workflow editor UI:
  - Name
  - Trigger
  - Branch selection
  - Step selection
- YAML preview modal
- Save and apply workflow flow

**Milestone**
- A non-technical user can create CI workflows via the UI

---

### **Week 8 — Dashboard and Integration Polish**
**Epic:** Frontend UI

**Goals**
- Tie features together into a cohesive experience

**Deliverables**
- Main dashboard:
  - Repository selector
  - Rule count
  - Workflow count
  - Recent activity feed
- Clear linkage between repositories, rules, and workflows
- Improved loading states and error messaging

**Milestone**
- Application feels cohesive and production-ready

---

### **Week 9 — Testing and Hardening**
**Epic:** Testing and Polish

**Goals**
- Prepare for live demo reliability

**Deliverables**
- End-to-end testing with real GitHub repositories
- Fix webhook edge cases and race conditions
- Handle:
  - Deleted workflows
  - Disabled GitHub App
  - Missing permissions
- Basic audit log view

**Milestone**
- System is stable and reliable for presentation

---

### **Week 10 — Documentation and Capstone Write-Up**
**Epic:** Documentation

**Goals**
- Complete academic and technical documentation

**Deliverables**
- Capstone report:
  - Problem statement
  - Research and justification
  - Architecture diagrams
  - Technical decisions
  - Screenshots
  - Challenges and solutions
  - Reflection (TPM → builder)
- Project README:
  - Overview
  - Features
  - Architecture
  - Setup instructions

**Milestone**
- Written deliverables finalized and polished

---

### **Week 11 — Presentation and Buffer**
**Epic:** Presentation and Final Prep

**Goals**
- Confident, stress-free presentation

**Deliverables**
- Slide deck
- Live demo walkthrough script
- Presentation practice runs
- Optional polish:
  - Minor UI refinements
  - Additional rule actions
  - Small analytics view (if time allows)

**Milestone**
- Ready for final capstone presentation

---

## ✅ Outcome

By the end of Week 11, this project delivers:
- A fully functional SaaS MVP
- A real GitHub App with automation capabilities
- A clean, demo-ready UI
- Complete technical and academic documentation
- A strong portfolio piece demonstrating full-stack engineering, DevOps automation, and system design


---

📘 Rules Engine behavior and guarantees:
- See `rules-engine/README.md` within API repo
