# Sidebar Routine prompt (paste into the single "daily report" routine)

Prereqs (set once, in Claude Code web settings — Claude can't do these):
- Attach the **`dennis643/charts`** repo to the routine's environment/trigger.
- Give the environment GitHub + Slack access.
- Use ONE routine (delete the second one).

One-metric-at-a-time prompt (ask candobro for each number separately, resetting
between questions so stale context can't bleed into the next answer):

```
Update the Candosa daily report (repo: dennis643/charts).

1. Make the repo current: run `git pull --rebase origin main`.

2. In Slack, DM candobro (channel D0BBBH668TE). Collect TODAY's numbers ONE METRIC AT A
   TIME — do NOT ask for the whole JSON in one message. candobro has been returning
   stale/erroring data when it carries context between answers, so RESET IT BEFORE EVERY
   QUESTION:
     a. Send `/reset` (if that doesn't clear it, send `reset` then `new chat`) and wait
        ~10s for candobro to confirm a fresh session.
     b. Ask for exactly ONE metric, e.g.:
        "For <YYYY-MM-DD>, reply with ONLY the integer for: active customers total
         (migrated + non-migrated). Number only, no words."
     c. Wait ~60s and read the DM. If candobro says it's rate-limited (or is silent),
        wait ~2–3 min and resend the SAME single question. Keep looping for up to ~2h
        total across all metrics. Only accept a clean integer as the answer.
     d. Record the number, then repeat from (a) for the next metric.

   Ask these metrics, in this order (each is its own reset → question → answer cycle):
   - activos:     active customers total (migrated + non-migrated)
   - onboarding:  clients still onboarding (no activity assigned yet)
   - migrados:    migrated clients imported but NOT yet activated (remaining)
   - noMigrados:  fully set-up / active non-migrated customers
   - trimestre:   current-quarter tax filings submitted or completed
   - migAct:      cumulative migrated clients activated (all-time)
   - migImp:      total migrated clients imported (all-time)
   - uploads:     distinct clients who uploaded >=1 document today
   - waClients:   distinct clients who messaged on WhatsApp today
   - waMessages:  cumulative all-time WhatsApp messages (running total)
   - taxDocs:     cumulative total tax documents in system (running total)
   - milestone:   one short sentence summarizing today (after the numbers, reset then ask)

3. Assemble the collected answers into a SINGLE one-line JSON object with EXACTLY these
   keys (integers only; milestone = one short sentence):

   {"date":"<YYYY-MM-DD>","activos":0,"onboarding":0,"migrados":0,"noMigrados":0,"trimestre":0,"migAct":0,"migImp":0,"uploads":0,"waClients":0,"waMessages":0,"taxDocs":0,"milestone":"…"}

4. Save the JSON to `today.json`, then run `bin/update-day.sh`. It merges the row for that
   date (activation rate is derived automatically), rebuilds index.html, commits, and
   pushes to main. If the push is rejected, run `git pull --rebase origin main` and
   re-run `bin/update-day.sh`.
5. Slack Dennis (channel_id U0AKELYLUCX): "✅ Candosa report updated for <date> →
   https://dennis643.github.io/charts/". If candobro never cleared after ~2h, Slack Dennis
   that instead and stop.

Pull ONE day only — never re-pull 30. A partial row is fine — omit any field candobro
can't give (never fabricate).
```
