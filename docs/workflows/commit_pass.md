# Commit Pass Workflow

## Goal
Based on your knowledge of the project, commit all changed files now in a series of logically connected groupings with super detailed commit messages for each and then push. Take your time to do it right. Don't edit the code at all. Don't commit obviously ephemeral files.

Make sure you review ALL beads (tasks) that have been recently completed, or are in progress, and try to isolate very small atomic commits that encapsulate at most one full task. Dont be afraid to stage single chunks inside files.


## Safe rules
- Never `git add -A`
- Stage explicit paths only
- Review `git diff --cached` before every commit
- If unsure a change is yours, do not stage it

## Suggested grouping
- 1 commit per coherent change (feature, refactor, test fix, docs)
- Keep beads updates with the relevant code changes

