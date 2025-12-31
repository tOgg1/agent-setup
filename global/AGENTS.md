# AGENTS.md, global rules

You are working in an environment where MULTIPLE agents often work in parallel in the SAME working directory.
Assume your `git status` may include other agents' changes.

## Most important rule

You may NOT delete any file or directory unless I explicitly give the exact command in this session.

This includes files you just created (tests, tmp files, scripts, etc.).
You do not get to decide that something is "safe" to remove.
If you think something should be removed, stop and ask. You must receive clear written approval before any deletion command is even proposed.
Treat "never delete files without permission" as a hard invariant.


## Golden Rules (Multi-agent shared workspace)

- Before editing ANY file: reserve it via MCP Agent Mail.
- Only edit files you have reserved (or have explicit permission for).
- Do NOT “clean up” unrelated code. Do NOT reformat large areas. Avoid drive-by changes.
- Never commit other agents' changes.
- Never run `git add -A`.

## Coordination: MCP Agent Mail (required)

Use Agent Mail for:

- announcing what you’re doing
- asking/answering questions
- reserving files (advisory leases)
- leaving an audit trail

Typical flow (align with Beads):

1) Pick ready work (Beads): `bd ready --json`
2) Reserve edit surface (Mail): `file_reservation_paths(project_key, agent_name, ["..."], ttl_seconds=3600, exclusive=true, reason="bd-123")`
3) Announce start (Mail): `send_message(..., thread_id="bd-123", subject="[bd-123] Start: <short title>")`
4) Work + post progress
5) Release reservations: `release_file_reservations(project_key, agent_name, paths=["..."])`
6) Close task: `bd close bd-123 --reason "Completed"`

(Exact tool names depend on MCP client integration; above matches mcp_agent_mail docs.)
More info: check the mcp_agent_mail README / CLI help.

## Memory: CASS + CASS Memory (recommended)

Before any non-trivial task:
- `cm context "<task description>" --json`

If you need instructions:
- `cm quickstart --json`
Health check:
- `cm doctor --json`

## Task System: Beads + BV

- New to Beads? Start with: `bd quickstart`
- Pick work: `bd ready --json`
- For prioritization/reporting: `bv --export-md ...`

## Quality Gate: UBS (required before claiming “done”)
Run before declaring completion:
- `ubs --fail-on-warning .`  (or relevant scope)
If environment issues:
- `ubs doctor`
To see recent setup/session info:
- `ubs sessions --entries 1`

## Git commits (your convention)
- Commit message: simple descriptive title (no conventional commits required).
- If there is a task id (e.g. bd-123), include it optionally for traceability.

### Safe commit procedure in shared working dir
1) `git status` (confirm you are not seeing unrelated changes staged)
2) `git add <explicit paths only>` (never `git add -A`)
3) `git diff --cached`
4) `git commit -m "Descriptive title"`
