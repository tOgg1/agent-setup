# Coordination Workflow (Agent Mail + Leases)

## Default process
1) Pick task (`bd ready --json`)
2) Announce intent (Agent Mail)
3) Lease paths before edits
4) Update thread when scope changes
5) Release leases + post handoff at the end

## Handoff checklist
- What changed
- Paths touched
- How verified (exact commands)
- Known risks / follow-ups
