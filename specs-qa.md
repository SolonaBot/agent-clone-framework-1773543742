Title: Agent-Clone QA Task

Goal

Implement QA subagent templates and automated test suites that validate the Lead/subagent orchestration. Provide integration tests that spawn a Marketing subagent, simulate a task, and assert the Lead receives the report and correctly processes a write-queue entry.

Deliverables

- /subagents/qa/runner.js — Node script that spawns a test subagent, runs a simulated Codex session (mock), and posts results to Lead API
- /tests/integration/spawn-and-report.test.js — integration test that uses sessions_spawn (or a test double) to validate end-to-end flow
- Mock Codex CLI adapter for CI so tests don't call live Codex during CI runs

Acceptance criteria

- `npm test` runs unit and integration tests locally (mocked network) and passes
- CI workflow includes the QA test step and fails fast if allocation or memory ACLs are violated

Notes

- Use mini model for drafting test skeletons but mock external service calls during CI.
- Keep tests deterministic and fast; avoid heavy builds on Catalina host during CI.

Run

- Commit files into /subagents/qa/ and /tests/, push, and include validation commands in final.md
