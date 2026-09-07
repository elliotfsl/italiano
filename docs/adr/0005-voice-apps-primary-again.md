---
status: accepted
date: metà settembre
---
# Voice lessons run in the ChatGPT or Claude app; the custom app is secondary

After one day of real use, Elliot judged the custom phone app too slow for conversation:
each exchange chains Scribe, a Sonnet call through the Agent SDK, and ElevenLabs TTS
(observed 15 to 40 seconds), and the in-page microphone was fragile on iOS Chrome. The
voice modes of the ChatGPT and Claude apps have none of that latency. Decided: the
primary lesson surface is a voice conversation in one of those apps, driven by a single
prompt (the Guida prompt, italiano/voce/PROMPT.md) that carries the ADR-0004 engine
rules: Turni as goals, off-script answered in character, steer back after two detours,
Italian first, benchmarks frozen, canonical post-mortem at the end. Content reaches the
chat two ways: Claude fetches the public mirror; ChatGPT gets the Dialogo pasted inline
(pacchetto v2, built by tools/build_voice_pack.py). The app (italiano-app on Railway)
stays deployed for cards, Anteprima reading, and the Post-mortem summary, but nothing new
is built there until the voice path proves insufficient. Its Railway cost is small; pausing
the service is the reversal.
