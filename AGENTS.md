# AGENTS.md — Project Agent Operating Manual

This repo is commonly worked on by MULTIPLE AGENTS IN PARALLEL, often in the SAME working directory.
Assume the workspace may change while you work.

If anything is unclear or conflicts with observed reality, STOP and ask rather than guessing.

---

## 0) Repo Quick Facts (EDIT PER REPO)

- Project: <name>
- Primary stack: <e.g. bun / node / python / go / rust>
- “How to run” (authoritative): `agent_docs/runbooks/dev.md`
- “How to test” (authoritative): `agent_docs/runbooks/test.md`
- “How to release/deploy” (if applicable): `agent_docs/runbooks/release.md`
- Repo map / key directories: `agent_docs/repo_map.md`
- Known pitfalls: `agent_docs/gotchas.md`

---

## 1) Non‑negotiables (read first)

### Multi-agent coordination is mandatory

- Before editing files, coordinate via **MCP Agent Mail**.
- Reserve the paths you will touch (leases) and announce intent.
- Only edit files you have reserved (or have explicit permission to share).

### No destructive actions without explicit approval

Do NOT run (or propose as a “quick fix”) without explicit user approval:
- `git reset --hard`
- `git clean -fd` (or variants)
- `rm -rf` (or any delete/overwrite command with broad scope)
- anything that deletes data, generated artifacts, or repo history

When in doubt: ask first.

### Keep diffs scoped

- No drive-by refactors.
- No mass reformatting.
- No “cleanup” outside the leased scope.

---

## 2) General workflow

1) Read the root README.md.
2) Read the index: `agent_docs/README.md`
3) Check MCP Agent Mail:
   - read inbox / recent thread activity
   - see if leases already exist on your target paths
4) Pick/confirm the task source (Beads is default):
   - `bd ready --json`
5) Announce intent (Agent Mail):
   - task id (if any), goal, target paths, expected outputs
6) Reserve files BEFORE editing (Agent Mail leases):
   - reserve the specific files/dirs you’ll change
7 optional) Pull relevant memory for non-trivial work:
   - `cm context "<what you are about to do>" --json`
8 optional) Before implementing something that might have been solved already:
   - `cass search "<keywords>" --robot --limit 5`
9) Implement the task you are assigned.
10) ALWAYS close the beads task before you hand off to the user again.

---

## 3) Coordination Protocol (MCP Agent Mail)

Use Agent Mail for:
- work announcements + status updates
- file reservations (leases)
- handoffs (“what changed / what to test / what’s next”)

Rules:
- Acquire leases before editing.
- If you hit a conflict, do not brute-force. Coordinate (adjust scope, wait, or get permission).
- Release leases when done.
- Post a final handoff message at the end of your work session.

If MCP Agent Mail tools are not available in your harness/runtime, tell the user immediately.

---

## 4) Task System (Beads is default)

Beads (`bd`) is the canonical task tracker.

- Quickstart to get a full overview over bd functionality: `bd quickstart`.
- Find ready work: `bd ready --json`
- Claim work: `bd update <id> --status in_progress --json`
- Create follow-ups: `bd create "Title" -t task -p 2 --json`
- Close work: `bd close <id> --reason "…" --json`

Project state:
- `.beads/` is authoritative and should be committed alongside related code changes.
- Do not hand-edit beads JSONL; use `bd`.

---

## 5) Quality Gate (Definition of Done)

Before you call work “done” or hand off:

1) Run the repo’s test/build/lint gates:
   - Follow `agent_docs/runbooks/test.md`
2) Run UBS bug scan (scope to changed files when possible):
   - preferred: `ubs --staged` (if staging is in use)
   - otherwise: `ubs --diff` (or `ubs .` for full scan)
3) Summarize:
   - what changed
   - how verified (exact commands)
   - risks / follow-ups

---

## 6) Git & PR Workflow (multi-agent safe)

### Commit messages
- Use a simple descriptive title. No strict column limits.
- Use a richer body explaining in detail what the commit does and why it exists.

### Default workflow: “Commit pass” (current)
In many projects, agents do not commit continuously.
Instead, we group changes into logical commits in a commit pass.

If you are NOT the integrator for this task:
- avoid committing/pushing unless explicitly requested
- leave a clean handoff summary + suggested commit grouping
- do not stage/commit unrelated changes

### Optional workflow: Continuous commits (allowed when explicitly chosen)
If the team chooses continuous commits for a task:
- keep commits small and coherent
- do not commit broken states unless explicitly agreed
- always stage explicitly (see below)

### Critical staging rule in shared working dirs
- NEVER use `git add -A` in a multi-agent shared working directory.
- Always stage explicit paths:
  - `git add path/to/file1 path/to/file2`
- Always verify:
  - `git diff --cached`
  - `git status`

### PR expectations
A PR description must include:
- What changed (bullets)
- Why (goal/context)
- How verified (exact commands + results)
- Risk areas / follow-ups
- Links/refs: Beads id(s) + Agent Mail thread (if available)

---

## 7) Tool Quick Reference (and how to learn more)

### MCP Agent Mail (coordination + leases)
- Use the MCP tools provided by your harness for:
  - inbox/thread reads, send message, acknowledge
  - file leases/reservations and releases
- Learn more:
  - If the harness supports listing MCP tools, do that.
  - Otherwise ask the user for the local integration commands.

### Beads — `bd` (tasks)
- Start here: `bd quickstart`
- Help: `bd --help`
- Ready work: `bd ready --json`

### Beads Viewer — `bv` (triage / planning)
- IMPORTANT: avoid interactive TUI unless explicitly requested.
- Prefer robot outputs (examples; adjust to your version):
  - `bv --robot-triage`
  - `bv --robot-next`
- Help: `bv --help`

### CASS — `cass` (cross-agent history search)
- IMPORTANT: never run bare `cass` (interactive).
- Prefer:
  - `cass health`
  - `cass search "<q>" --robot --limit 5`
  - `cass capabilities --json`
  - `cass robot-docs guide`
- Help: `cass --help`

### CASS Memory — `cm` (procedural memory)
- Before non-trivial tasks:
  - `cm context "<task>" --json`
- Help: `cm --help`

### Ultimate Bug Scanner — `ubs` (bug scan)
- Preferred: `ubs --staged` (or `ubs --diff`)
- Full scan: `ubs .`
- Help: `ubs --help`

---

## 8) Keeping agent docs healthy (required habit)

If you learn something that will save future time, update `agent_docs/`:
- new command or workflow -> update runbook
- architectural constraint -> add a decision doc
- recurring failure mode -> add to gotchas

Keep `AGENTS.md` short and operational.
Put repo-specific, evolving knowledge in `agent_docs/`.
