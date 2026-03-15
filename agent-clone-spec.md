Title: Agent-Clone Framework (Lead + Subagent orchestration)

Overview

Create an "agent-clone" framework that provides a Lead orchestrator and spawnable Department subagents. The Lead is the authoritative controller that assigns tasks, enforces memory-write policies, and approves code changes via Codex CLI. Subagents are ephemeral workers (marketing, QA, infra, dev) spawned via sessions_spawn and execute work using the Codex CLI or other allowed tools. The deliverable will be a repo scaffold, CI workflows, Codex task specs, and operational docs so we can reproduce and extend with minimal manual steps.

Goals (high-level)

- Provide a reproducible repo scaffold for agent clones and Lead.
- Implement a minimal Lead service (node script) that can spawn subagents via sessions_spawn and maintain a write-queue for memory edits.
- Implement subagent templates (Marketing, QA, Infra, Dev) that integrate with sessions_spawn and follow read-only/approved-write memory rules.
- Provide CI (GitHub Actions) that builds clone artifacts, publishes them as artifacts, and can be used by shadow lanes.
- Provide clear security & access controls: Lead-only authoritative memory writes; subagents read semantic memory; all code changes must be performed via Codex CLI tasks.
- Acceptance criteria: working repo scaffold, one successful Codex CLI task that creates a simple "hello" clone, documented spawn workflow, automated tests for spawn & memory ACL, and a Playbook outlining operation.

Deliverables

1. Repository: agent-clone-framework
   - README.md (overview + quickstart)
   - /specs/ (JSON/YAML specs for clones and tasks)
   - /ops/ (orchestrator docs, memory ACLs, runbooks)
   - /lead/ (Lead service: node CLI + simple REST control API)
   - /subagents/marketing, /subagents/qa, /subagents/dev, /subagents/infra (templates + run scripts)
   - /codex/tasks/ (Codex task specs used to implement each component)
   - .github/workflows/build.yml (CI to build and package clone artifacts)

2. Codex Task Specs (extensive):
   - Task A: Scaffold repository + file layout + initial docs
   - Task B: Implement Lead service (sessions_spawn wrapper, memory write-queue API, REST endpoints, auth)
   - Task C: Implement one example subagent (Marketing) that accepts a task, posts a message in a Discord channel, and reports results back to Lead
   - Task D: Implement CI workflow that builds the clone artifact and uploads it
   - Task E: Tests: unit tests for Lead API, integration test that spawns a subagent and confirms reporting

3. Operational docs & runbook:
   - Memory access model (exact archive read/write rules, semantic index read-only for subagents)
   - How to spawn a subagent and approve a memory write
   - Cost-control guardrails (limit Codex model selection, per-task caps)
   - Disaster recovery & rollback for clones

Technical design notes (do not change without approval)

- Memory model
  - Exact Archive: canonical DB at ~/openclaw/persist/state/exact-archive.db. Only Lead may write. Subagents request writes via Lead's write-queue.
  - Semantic Index: read-only for subagents. Reindex performed by Lead-approved jobs.
  - All memory write requests from subagents stored in write-queue table; Lead evaluates and commits or rejects.

- Spawn model
  - Use OpenClaw sessions_spawn (runtime="subagent") to spawn subagents. Provide each subagent a short-lived token and a JSON task payload.
  - Subagents use Codex CLI to implement any code tasks. Lead only approves merges (Lead triggers Codex CLI job or reviews produced patch).

- Security & ACL
  - Secrets stored in ~/openclaw/persist/config/secrets.env; Lead has access. Subagents run with reduced environment (no secrets) unless owner's transient approval is granted.
  - Codex tasks that modify production repos must include a one-line Telegram approval phrase; the codex-loop-submit.sh wrapper enforces this. The Lead automates the approval check if configured.

- CI / Artifact
  - GitHub Actions workflow builds clone artifacts in a shadow lane macOS/linux runner; artifacts published for download and for use in shadow lane.

Acceptance tests

- Unit: Lead API starts, returns health, enqueue write-requests, can mark approved.
- Integration: spawn Marketing subagent via sessions_spawn -> subagent posts test message to Discord (or prints to stdout) -> Lead receives report and marks write approved.
- Codex pipeline: codex-loop-submit.sh runs Task A and creates repo scaffold; test passes if repo contains expected files and CI workflow present.

Execution plan for Codex (how to run)

- Create a Codex task file per Task A..E, each with:
  - Context: repo path, templates, helpful examples
  - Acceptance criteria: file names, test commands, example outputs
  - Security notes: do not modify secrets.env
- Submit via: ~/openclaw/bin/codex-loop-submit.sh --label "agent-clone-framework-task-A" < specs/task-A.md

Rollout and next steps

- After Task A completes and scaffold repo exists, run Task B/C (Lead + Marketing subagent). Use sessions_spawn to validate; collect logs.
- Once stable, create additional subagent templates and a small dashboard UI to visualize active subagents and tasks.

Constraints & guardrails

- All coding through Codex CLI (codex-loop-submit.sh). No ad-hoc local edits.
- Lead coordinates all memory writes. Subagents only request writes.
- Respect disk and cost constraints: avoid heavy local builds; prefer CI artifact builds in shadow lane.



