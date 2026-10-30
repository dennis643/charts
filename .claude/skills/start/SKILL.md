---
name: start
description: Start-of-session pickup. Reads the latest work summary posted by /end in #designops (and WORK_SUMMARY.md) and reports where things were left off. Use when the user types /start or asks to pick up where the last session ended.
---

# Start session

1. Read the most recent summary posted by `/end`: `slack_read_channel` on `#designops` (ID `C0AR1GJ8G1M`), newest first. Pick the latest message starting with "Work summary".
2. Read `WORK_SUMMARY.md` in the repo root as a cross-check. If the two differ, trust git and say so.
3. Check the current state: `git fetch origin`, `git status -sb`, `git log --oneline -10`.
4. Reply briefly: what was done last time, the current branch state, and any open items or uncommitted/unpushed work. Ask what to work on next; do not start changes unprompted.
