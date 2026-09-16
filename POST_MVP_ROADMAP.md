# Post-MVP Roadmap

The capstone MVP (see `README.md`) is complete — all 7 planned epics shipped.
This document tracks what comes next, now that development is ongoing/
free-flow rather than deadline-driven.

Related repos:
- [ad490-workflow-automator-api](https://github.com/JesseCaddell/ad490-workflow-automator-api)
- [ad490-workflow-automator-web](https://github.com/JesseCaddell/ad490-workflow-automator-web)

---

## Sequencing

Ordered by dependency, not by importance — each phase assumes the ones
above it are done. Phases 1–6 are the originally-scoped post-MVP backlog;
Phase 2 was added because it's the natural next step once Phase 1 exists,
even though it wasn't in the original list.

### Phase 1 — GitHub App auth, installation-scoped

**Status:** not started. Verified: the API currently only has app-level JWT
auth (`makeAppOctokit()` in `src/github/appAuth.ts`) — nothing exchanges an
`installationId` for an installation access token. Everything below that
touches real GitHub data depends on this existing first.

- Add `getInstallationOctokit(installationId)` using `createAppAuth` with
  `{ type: "installation", installationId }`.
- Cache tokens (installation tokens expire ~1hr) rather than re-exchanging
  on every call.

### Phase 2 — Real action execution

**Not in the original post-MVP list — added here because it's the natural
next step once Phase 1 lands, and arguably the highest-value milestone:**
it's the difference between a demo (logged stubs) and a SaaS that actually
does things.

- `addLabel` / `addComment`: currently stubbed (log-only) — wire real
  Octokit mutations using the Phase 1 installation client.
- `removeLabel` / `setProjectStatus`: currently not even stubbed — implement
  from scratch.

### Phase 3 — Dynamic GitHub data for action inputs

**Goal:** replace free-text action params with repo/project-backed
dropdowns. Sequenced after Phase 2 because by then you know what a "valid"
label/project/status looks like end-to-end, instead of designing the API
shape twice.

**UI changes** — Workflow editor → Action card inputs:
- `setProjectStatus`: Project dropdown + Status dropdown (project drives
  status options)
- `addLabel` / `removeLabel`: Labels dropdown instead of free text
  (searchable, later)

**API additions** (scoped to installation + repo):
- `GET /api/github/projects` — list Projects v2 available to the repo/user
  context
- `GET /api/github/projects/:projectId/status-options` — list status field
  options (single-select field, usually "Status")
- `GET /api/github/labels` — list repo labels

**Implementation notes:**
- GitHub GraphQL for Projects v2 + field options; REST is fine for labels.
- Cache responses (short TTL) to avoid rate limits and keep the UI snappy.
- Store the selected `projectId` per action step inside the workflow step's
  params, so each workflow stays deterministic.

### Phase 4 — Multi-repo support

Replace the env-var-derived demo scope with the real thing.

- Replace env repo options with authenticated GitHub installation listing.
- Support multi-installation + multi-repo switching.
- Persist selected repo per user.
- Enforce repo-level authorization in both frontend and API.

**Open question, not yet decided:** "enforce repo-level authorization"
implicitly requires *some* notion of an authenticated user/session, which
doesn't exist yet — the web app currently assumes a single trusted demo
user. Before building this phase, decide: is this still single-owner
(authorization just means "use the right installationId"), or is real
multi-user login actually wanted? That decision changes what "enforce"
means here, and whether a real-auth/RBAC track (see below) needs to move
up in priority first.

### Phase 5 — API hook improvements

Sequenced after Phase 4 — not very valuable until scope-switching between
real repos is something that happens often.

- Per-`scopeKey` cache for workflows + single-workflow fetches, to avoid
  refetching on scope switch.
- Deduplicate in-flight requests per `scopeKey` (share promise, cancel/
  ignore stale requests).
- Optionally wire a global loading bar to hook network activity.

### Phase 6 — Global loading UX strategy

Last, deliberately: design it around the real latency introduced by Phases
1–5 (auth handshake, installation token retrieval, GraphQL queries,
cross-page navigation), not around guesses made before those flows exist.

Before implementing a global loading indicator:
- Identify what actually qualifies as "global loading" (the list above).
- Decide between: route-level `loading.tsx` (preferred for App Router),
  local component loading states (tables, editor), or a global topbar
  progress indicator (only if multiple concurrent async flows genuinely
  justify it).
- Avoid: always-on animated indicators, a global loading store unless
  multiple async layers justify it, UX noise when responses are sub-200ms.

Goal: loading UI should represent real latency, not simulate activity.

---

## Additional gaps (found auditing the code/docs, not yet scheduled)

These showed up while reviewing `CLAUDE.md` and `docs/*.md` in both repos —
they're real gaps, but on a different track (reliability/scale) than the
GitHub-integration track above, so they're not slotted into the numbered
sequence yet:

- **Persistence** — both the rules engine and workflow engine are in-memory
  only; all data is lost on restart. No DB, no migrations.
- **Reliability** — no retries, no rollback/transaction semantics. Action
  failures don't halt evaluation, but they also aren't retried.
- **Engine capability** — no branching/conditional logic in workflows,
  single trigger per workflow, 1–25 step cap, no scheduled/time-based
  triggers, no cross-repo or org-level workflows.
- **Rules-engine vs. workflow-engine consolidation** — the condition-based
  rules engine and the newer workflow builder currently run in parallel off
  the same normalized event. Whether they should eventually merge into one
  engine is an open product question, not something to do casually — see
  the API repo's `CLAUDE.md`.
- **Real user auth / RBAC** — distinct from GitHub App installation auth.
  Currently absent entirely; relevant to the open question in Phase 4
  above.
