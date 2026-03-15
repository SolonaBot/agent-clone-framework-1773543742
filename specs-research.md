Title: Agent-Clone Research Task

Goal

Perform a deep research pass on Paperclip and related agent-orchestration platforms to extract implementation patterns, integration points, and risks. Produce a concise report (Markdown) summarizing:
- Architecture primitives (org chart, heartbeat, task checkout, budget enforcement)
- Relevant components in Paperclip codebase (API server, agent adapters, task scheduler, DB schemas)
- Integration points to OpenClaw (sessions_spawn, memory model, Codex-runner, semantic index)
- Security model and secrets handling best practices
- CI/deployment patterns for shadow lane and artifact builds
- Risks and mitigation suggestions for running many concurrent Codex sessions on macOS Catalina

Acceptance criteria

- A Markdown report at docs/research/paperclip-integration.md inside the target repo containing each section above, with code references (file paths) and minimal examples.
- A short README snippet showing how Paperclip concepts map to our Agent-Clone framework.
- A 5-item prioritized risk list with recommended mitigations.

Notes for Codex

- Use github-copilot/gpt-5-mini for reasoning & code snippets.
- Do not touch secrets.env.
- Prefer reversible, minimal edits in the repo.
- Run quick validations: list the top-level directories in the cloned Paperclip repo and cite 3 files that implement the scheduler and agent adapters.

Run instructions

- Implement as codex-task-run in the repo root. Output files under docs/research/. Commit changes and push to the repo. Provide a final summary in the task final.md with the file list and highlights.
