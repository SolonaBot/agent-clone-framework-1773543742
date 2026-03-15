Title: Agent-Clone Spec Task

Goal

Produce exhaustive Codex task specs for implementing the Agent-Clone framework components. This is the canonical spec set that downstream Codex runs will implement. Deliver detailed task files for: Lead service, Subagent templates (Marketing, QA, Dev), Memory ACLs, CI workflow, and Tests. Each task file should include acceptance criteria, validation commands, and examples.

Deliverables

- /specs/task-lead.md — Lead service: API, session_spawn wrapper, write-queue schema, example REST endpoints, unit tests
- /specs/task-subagent-marketing.md — Marketing subagent template: how to accept tasks, run Codex sessions, report back to Lead
- /specs/task-subagent-qa.md — QA subagent template: run tests, capture logs, file bug reports into the signup repo
- /specs/task-ci.md — GitHub Actions workflows for building/publishing clone artifacts and for running integration tests in the shadow lane
- /specs/task-tests.md — Acceptance & integration test definitions and scripts

Acceptance criteria

- Each spec file (above) exists under /specs/ and is >500 words, includes concrete example commands and JSON payloads, and ends with explicit validation commands (e.g., `npm test`, `node tests/run-integration.js`).
- A top-level implementation plan (ops/IMPLEMENTATION_PLAN.md) shows the execution order and estimated runtime/cost per task.

Notes

- Use the mini model for drafting.
- Ensure tasks are self-contained so Codex runs can pick them up and execute without human prompts.
- Do not modify secrets.env or push secrets.

Run

- Commit spec files to /specs/ and push. Finalize with a short final.md summarizing filenames and high-level steps.
