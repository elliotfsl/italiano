# Sessioni portatili (pacchetti per claude.ai)

Built for: Elliot, so he can run spoken sessions in claude.ai voice mode. Last built: 2026-07-26.

Why this exists: Claude Code can hold program state but cannot speak. claude.ai voice mode speaks Italian but cannot write to this repo. A pacchetto bridges them. A repo session generates a self-contained markdown file; Elliot opens a brand new claude.ai chat, attaches or pastes it, and runs the session out loud; at the end that chat prints a post-mortem; he pastes the post-mortem back into a repo session, which folds it into `STATUS.md`, `lessico.md`, and `sessioni/`.

Since 2026-08-30 the PRIMARY voice path is the public mirror instead: Elliot sends a fresh
claude.ai chat one line pointing at `raw.githubusercontent.com/elliotfsl/italiano/main/README.md`
(text first, so it loads, then voice mode). The chat reads live STATUS and the day's dialogue,
so nothing goes stale and nothing needs generating. Packs remain the offline fallback for when
fetching is unavailable. The return path is identical in both modes: the post-mortem paste-back
below is the only write.

Packs live in `pacchetti/` as `gNN.md`. They are disposable: once a post-mortem is ingested, the pack has done its job.

## Generating a batch

Default batch is 3 packs. Before generating, run Avvio (PROTOCOL.md section 1) so the batch is built from real state: current giorno and fase, due cohorts, open error patterns, unticked scenarios, and any Chiamate vere rehearsal window.

Honest limitation to respect: within a batch, only the first pack knows current state. Packs 2 and 3 are built blind to what happens in pack 1. So put the highest-value recycling and the tightest error-pattern work in pack 1, and make later packs lean more on fresh material and broad review. Never generate more than a week ahead.

Every pack is fully self-contained. The claude.ai chat has no repo access, no memory, and no other file. Anything the session needs must be inside the pack.

## Pack anatomy

Four parts, in this order:

1. **Istruzioni** (for the claude.ai Claude): the condensed method. Voice-first turn rules, recasts not corrections, dictation tolerance, the "?" hatch, and the instruction to never read markdown syntax, headers, or the pack's structure aloud. Also: do not dump the whole session plan at the start, just begin the conversation.
2. **La sessione**: warm-up topic, 3 role-play scenarios with their complications, one pressure drill, and 2 spare scenarios for extra energy. Each scenario gets a scene line, Claude's opening line verbatim, the complication to introduce mid-scene, and the closing beat.
3. **Post-mortem**: the exact template the chat must print at the end, in a single copyable code block. This is the return path, so its shape is fixed.
4. **Solo per Claude**: the recycle list (due vocab from cohorts N-1, N-3, N-7, N-14 plus watch list) that the chat should weave in without showing, marked so Elliot skips it. New target vocab is NOT hidden; it appears in part 2 so he can see the day's goal.

Fase settings carry over from PROTOCOL.md section 3 and the phase specs in programma.md: turn length caps, gloss policy, complication intensity. State them literally inside each pack rather than referencing them, since the chat cannot read those files.

## Il modello di post-mortem (canonico, metà settembre)

This is the return path for BOTH portable modes, pack and mirror. A pack embeds a copy; a
mirror-driven chat reads it from here. The chat prints it at the very end, filled in, inside
ONE code block, and tells Elliot to paste it into his Claude Code session. Keep the labels
exactly as written, because ingestion below keys on them. Leave a field as `niente` rather
than inventing content.

```
POST-MORTEM ITALIANO
Giorno: <NN, da STATUS.md>
Data: <YYYY-MM-DD della sessione>
Modo: <pacchetto portatile | specchio pubblico | app>, voce
Fase: <da STATUS.md>
Tema: <dialogo o benchmark di oggi>

BENCHMARK <N> (solo se oggi era dovuto, altrimenti cancella questo blocco intero)
| Scenario | Compito | "?" | Riparazioni | Compr. | Sciolt. | Corr. | Lessico |
|---|---|---|---|---|---|---|---|
| Ristorante | sì/no | n | n | 1-5 | 1-5 | 1-5 | 1-5 |
| Autonoleggio | sì/no | n | n | 1-5 | 1-5 | 1-5 | 1-5 |
| Bar | sì/no | n | n | 1-5 | 1-5 | 1-5 | 1-5 |

Citazioni (2 o 3 sue frasi, verbatim):
- "..."

Giudizio complessivo (un paragrafo, onesto):
...

Confronto col benchmark precedente (salta al run 1):
...

SCENARI FATTI
- <file di dialoghi/ o riga della Banca scenari, uno per riga, solo quelli davvero corsi>
- <per ogni scenario: anteprima sì/no, prova 1 sì/no, prova 2 sì/no, ripasso a freddo sì/no>

COM'È ANDATA
<2 righe, oneste>

ERRORI TOP 3 (dalla prova 2, non dalla prova 1)
- <pattern>: "<quello che ha detto>" -> "<corretto>"

NOTA GRAMMATICALE
<una, solo se è emersa>

LESSICO NUOVO
| Italiano | Inglese | Nota |
|---|---|---|
<6-10 voci incontrate davvero oggi, non la lista teorica del file>

RIPASSO ANDATO MALE
<parole delle sessioni passate che non è riuscito a produrre, o "niente">

PROSSIMA VOLTA
<un filo aperto, o il dialogo consigliato per la prossima sessione>
```

Scoring definitions, so the numbers stay comparable across the program: Compito means he
completed the transaction without being rescued in English. Riparazioni counts the times he
asked for repetition or restated himself to fix a breakdown; early in the program this going
up is good, later it should fall. Comprensione is how much he understood at delivery speed.
Scioltezza is flow and turn length. Correttezza is grammar and form, ignoring speech
artifacts. Lessico is range and precision.

## Ingesting a post-mortem

When Elliot pastes one or more post-mortems into a repo session, treat it exactly like a completed session, with the writes from PROTOCOL.md section 6 in the same crash-safe order:

1. `lessico.md`: append a cohort per session day from the post-mortem's vocab list, plus any "(ripasso)" re-entries for items he flopped. Watch-list additions on third flops.
2. `sessioni/YYYY-MM-DD-gNN.md`: create the log directly in its filled form (there is no stub, since the session ran elsewhere). Note "sessione portatile" in the header line.
3. `programma.md`: tick the scenarios the post-mortem says were actually run.
4. `STATUS.md`: full rewrite. Counters, streak, error patterns, benchmark row if a benchmark ran, prossima volta hook.

Dates: the post-mortem carries the date the session actually ran. Use that, not today's date, so the calendar and the day counter stay honest. If several post-mortems arrive at once, ingest them oldest first, one cohort each.

If a post-mortem is partial or malformed, ingest what is there and note the gap in the log rather than inventing content. A missing vocab list is better recorded as missing than filled with guesses.

A post-mortem with `Modo: app` counts as a session day exactly like `pacchetto portatile` or
`specchio pubblico` (decided metà settembre): same ingestion steps, same day counter, same streak.

## Choosing a mode day to day

Both modes are legitimate; they train different things. Repo sessions give precise written correction and full bookkeeping. Portable packs give spoken practice with real listening pressure. A reasonable rhythm is to run packs when he wants to speak or is away from the machine, and repo sessions when he wants accuracy work, but nothing enforces a pattern.

Benchmarks are better run spoken, since spoken performance is the thing being measured, so a portable pack or a mirror-driven voice chat is the preferred mode for them, not the exception. The one condition is that the post-mortem comes back carrying the scoring block from `programma.md` verbatim; without it the run measured nothing. The mirror README states the same rule, and `STATUS.md` is the file that says whether a benchmark is due today. If those three ever disagree, `STATUS.md` wins and the disagreement gets fixed in the same session.
