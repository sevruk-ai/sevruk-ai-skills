---
name: always-check-current-date
description: "Use when a date or time matters: verify it before replying and stamp the answer."
version: 1.2.1
author: Vladimir Sevruk
license: MIT
metadata:
  hermes:
    tags: [date, time, timestamp, clock, timezone]
---

# Always Check the Current Date and Time

Before every reply, check the current date and time and add a timestamp at the end of the reply. These are the only facts that go stale while a conversation is still running: a chat can last days or weeks, and the date from the beginning of the chat stays in the agent's head as "today". The date matters for documents, deadlines and "this is out of date" judgements; the time matters for intervals shorter than a day, deadlines and the exact moment in logs.

## Steps (every reply)

1. **Before you start reasoning** (not after the text is written), get the current date and time in the user's zone, in the format the user writes dates in:
   - Russia and most of Europe (DD.MM.YYYY, 24-hour): `date "+%d.%m.%Y %H:%M (%Z)"`
   - the United States (MM/DD/YYYY, 12-hour): `date "+%m/%d/%Y %I:%M %p (%Z)"`
   - neutral ISO 8601: `date "+%Y-%m-%d %H:%M (%Z)"`
   - Windows PowerShell: `Get-Date -Format "dd.MM.yyyy HH:mm (zzz)"` — change the format string to the user's convention.
   Do it first — otherwise the stamp is right while the deadline arithmetic inside the text is not.
2. Compare the result with the date known from context: the system message, the start of the chat, the latest replies.
3. If they match, use them. If they differ, tell the user explicitly and use the value from the fresh command: a date held in context is a snapshot taken when the context was created, and if the session resumes days later, it is no longer "today". If the fresh value jumped days ahead, that is real elapsed time, not a failure.
4. End every reply with a stamp: italic, on a separate line, in the date and time format the user uses (the machine locale or the way the user writes dates). Examples: `*08.11.2026 19:43 (MSK)*` for Russia and Europe, `*11/08/2026 7:43 PM (EST)*` for the United States, `*2026-11-08 19:43 (UTC)*` for ISO. ZONE is what `%Z` returns (`MSK`, `CET`, `UTC`…). No comma, not bold, do not comment on the stamp. The first reply in a chat carries the stamp both at the top and at the bottom.
5. Never name the weekday from memory — take it from `date "+%A"` (PowerShell: `Get-Date -Format "dddd"`).

## Time zone

No time zone is hardcoded by default: the skill uses the zone the machine runs in, and `%Z` puts it into the stamp. If the user has agreed on a specific zone, set it explicitly — `TZ=<zone> date "+%d.%m.%Y %H:%M (%Z)"` — and stamp with that zone.

## Sources of the date and their reliability

- `date` — the clock of the machine the agent runs on. The primary source; clocks in containers and virtual machines can be off, so re-check when running outside a regular machine.
- The date in the system context, in previous replies and in memory records is a snapshot of the past. The longer the chat lives, the further it drifts from reality.
- An external source independent of the machine is the HTTP `Date` header (UTC time): `curl -sI --max-time 10 https://example.com | grep -i '^date'`, then compare it with `date -u`. A difference of more than a minute means the clock is off; in that case use the external time for files, records and stamps. The output of `date` is the machine's time, not "server time": only an external source gives server time.

## When verification is mandatory

- a date or time in a document, report, spec, email, file name, log entry;
- "today", "yesterday", "in N days", "overdue", the age of a publication;
- deadlines, expiry dates, schedules, weekday;
- any chat that lives longer than a day, and any session resumed after a pause: the latest stamp in the chat is not "now".

## If a date was already stated incorrectly

Say so directly, list the affected conclusions and files, propose a fix — and redo the files when the user asks. Never leave wrong dates in place.

## Typical mistakes

- Taking the date from the start of the chat and treating it as today's a week later: a publication dated 3 August is declared "in the future" because the chat says "today is 30 July".
- Deriving the date from memory records or previous replies: documents go out dated several days earlier than they actually are.
- A container with a wrong clock: `date` returns 10 August when it is really the 14th — and the wrong date lands in project files.
- Calling the output of `date` "server time"; it is the machine's clock, and server time comes only from an external source.

## Related skills

Works together with `project-manager` (if installed): the date and time of a `_JOURNAL.md` entry come from the `date` command, not from memory.
