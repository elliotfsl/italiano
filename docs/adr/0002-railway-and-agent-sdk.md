---
status: accepted
date: metà settembre
---
# Railway host, Claude Agent SDK on the subscription login for the grader

Grader and Ponte calls go through the Claude Agent SDK (Sonnet 5, no tools) authenticated
with a long-lived subscription token, not an API key, because Elliot wants the app to draw
on the Max allotment. The SDK spawns the Claude Code CLI as a subprocess, which needs a
long-lived Node process, and a Turno chains Scribe, grader, and TTS past serverless
timeouts. Both point to Railway over Vercel.

Consequence: a prior finding (Hermes, third-party OAuth) showed subscription logins can bill
to extra-usage credits. First graded Turno is followed by a usage-page check. If it bills as
extra usage, the grader route switches to an API key by env var, no code change.

Amendment metà settembre: results and the TTS cache live in a Railway Postgres in the same
project, not Supabase. The Supabase org had hit its free project limit, and sharing a
business project's service key with a personal app was the wrong trade.
