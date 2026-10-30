# Work summary

Branch: `claude/report-import-ig5ou8`

- Imported the Candosa daily report as charts
- Added a daily workflow to refresh report data and rebuild charts (`DAILY_UPDATE.md`)
- Merged the 2026-07-01 single-day report from candobro
- Added a one-command daily update routine (candobro -> merge -> push)
- Routine prompt now retries candobro until it answers (bounded, ~2h)
- Added `ROUTINES.md` with the single full-report routine prompt
