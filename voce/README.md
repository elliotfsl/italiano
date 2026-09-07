# Guida a voce: how to run a session

Three files do the work. `PROMPT.md` is the core prompt, the rules the Guida follows. `MODE-CLAUDE.md` and `MODE-CHATGPT.md` are the mode blocks. You always paste the core plus exactly one mode block, never both.

STATUS.md decides everything about the day: the giorno counter, the phase, whether a benchmark is due, and which scenes run. You don't tell the Guida any of that. Either it reads STATUS itself, or the pack hands it a STATUS excerpt.

## Route A, the Claude app

This is the primary route since the public mirror went up.

Open a new chat and send it in text mode, not voice. Paste `PROMPT.md` then a blank line then `MODE-CLAUDE.md`, and send. If you'd rather not paste it every time, put the same two files in the Project's instructions and skip to the boot message.

With Project instructions set, the boot message is one line:

```
Guida: run today's session from the Project instructions. Fetch STATUS.md and the files the MODE block names, say nothing about them, and open in Italian, in character.
```

Either way, wait for the reply. It should be one short Italian sentence and nothing else. That's the tell that the fetch worked. If it summarizes what it read, or greets you as an assistant, start over.

Then tap voice in that same thread and answer out loud. Don't start voice from a blank chat. Voice mode fetches badly, so the text turn is what loads the day's material.

## Route B, the ChatGPT app

Build the pack first:

```
python italiano/tools/build_voice_pack.py --dialogo <slug>
```

Add `--benchmark` when STATUS says one is due. The script writes `pacchetti/gNN-v2.md`. `PACK-SPEC.md` is what it assembles and in what order.

Open a new chat in the ChatGPT app, paste the whole pack as the first message, as text, and send. Wait for the first Italian line to appear. Then tap voice in that same thread and answer out loud. Starting voice from the home screen gives you a conversation with no pack in it, and voice mode can't fetch anything.

If you have a ChatGPT Project and its instructions field takes the length, you can put the core plus the ChatGPT mode block there and paste only the rest of the pack each day. Don't use the two custom instruction boxes. They cap around 1,500 characters each and will cut the core in half.

## What you can say out loud

- "opzioni" or "aiuto", three ways to answer the moment you're in. Say the word alone, nothing around it.
- "in inglese", the English of the last line, once. It's counted.
- "ripeti", say it again.
- "più lentamente", slower.
- "pausa", it stops talking and waits for you.
- "basta per oggi", end the session.

"Basta così, grazie" inside an order still means "that's all", so you can close an order normally without ending the session.

On a benchmark day it refuses "opzioni" and "aiuto" on purpose. "in inglese" still works once per scene and gets scored.

## Getting it back into the repo

At the end it drops character, gives you two or three sentences on what worked, then walks the errors one at a time and waits for you to say each fix back. After that it prints the post-mortem as one code block and tells you it's on screen. It won't read the block aloud.

Copy that block and paste it into a Claude Code session in primatine194. That paste is the only thing that writes to the repo. Everything else the session did is disposable.

If it's in a voice mode that can't write without speaking, it'll say "Passa al testo e scrivi post-mortem". Type that and the block appears.

## If the fetch fails

Route A retries once, then tells you in one line and carries on with what it has. The post-mortem still comes out, with `niente` in the fields it couldn't fill. On a benchmark day, if the frozen lines are the thing that didn't load, it won't invent them. It says so and runs the day's scene instead. Rerun the benchmark from a working session rather than accepting an invented one.
