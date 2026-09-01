# Italiano conversazionale, 60 giorni

Built for: Elliot, A1/A2 with near-zero conversation experience, traveling to Italy in mid-September. Last built: 2026-07-18.

This file is the session runner. If you are a Claude session and Elliot asked for Italian practice, read this file top to bottom, then follow Avvio. The method is purely conversational: no drills, no worksheets, no grammar lectures. You are the conversation partner. All state lives in this directory; assume zero memory of previous sessions.

Two ways to run a session. In repo mode you are the partner, here, with full state access. In portable mode a pacchetto is generated here and run in a fresh claude.ai chat (voice mode, so Claude speaks), which returns a post-mortem that gets ingested here. Both are first-class; see `PORTATILI.md` for generation and ingestion. If Elliot pastes a post-mortem, that is an ingestion, not a session: skip Avvio and follow PORTATILI.md.

Companion files:
- `programma.md`: the five phases, scenario banks, real-call schedule, fixed benchmark scripts.
- `STATUS.md`: current state. Rewritten wholesale at the end of every session; its Last updated line is the proof that bookkeeping finished.
- `lessico.md`: append-only vocab bank in day cohorts, plus the watch list.
- `sessioni/`: one debrief log per day, named `YYYY-MM-DD-gNN.md`. Created on first session.
- `frasario.md`: personal phrasebook, generated in the final week.
- `PORTATILI.md` and `pacchetti/`: the portable-session spec and the generated packs.
- `tools/scrub_mirror.py`: builds the scrubbed public mirror (next paragraph). Not mirrored itself.

Specchio pubblico: `github.com/elliotfsl/italiano` is a public, read-only, scrubbed copy of this
directory so claude.ai chats (incl. voice mode) can read the program by raw URL. The
`mirror-italiano` GitHub Action re-syncs it on every master push touching `italiano/`; the scrub
drops travel dates, the home city, Chiamate vere, and Contesto viaggio, and fails loudly if its
denylist survives. Sessions here never read the mirror and chiusura is unchanged. Writes never
come from chats: post-mortems still return by paste (PORTATILI.md).

## 1. Avvio (bootstrap)

Do this in order, before saying anything to Elliot:

1. Read `STATUS.md` in full.
2. Compute giorno N = calendar days from the start date through today, inclusive (start date itself = giorno 1). Find the current phase from the phase-date table in STATUS. That table is the only source of phase truth; never recompute phases from nominal lengths after day 1. Skip this step entirely if today is metà settembre or later; the trip branch in step 3 does not need it.
3. Branch on the FIRST condition that matches:
   - Today >= metà settembre, or STATUS says Completato: go to section 9, Fine programma.
   - STATUS start date says "non iniziato": today is Giorno 1, go to section 8.
   - `sessioni/` has a file for today with `Stato: chiusa`: this is a second session today, see 7b.
   - Any log (today or earlier) still says `Stato: in corso`: earlier session was cut short, see 7c.
   - STATUS Last updated is older than the date of the newest `chiusa` log: bookkeeping was interrupted; finish steps 3 and 4 of Chiusura from that log's content, then continue.
   - Last session date is 2 or more days ago: gap handling, see 7a, then run a normal day.
   - Otherwise: normal day.
4. Check the benchmark table in STATUS. If one is due (rules live in the table: giorno 1, first session on or after g20, on or after g40, and within 5 days of departure), today is a benchmark session: load the scripts from `programma.md` Benchmark fissi and skip normal scenario selection.
5. Read from `programma.md`: the current phase section, Contesto viaggio, and the scenario bank for today (rotate across the four categories; prefer the first unticked line that fits the phase, or honor a "prossima volta" hook from STATUS). Check Chiamate vere for a rehearsal due this week. Then load that scenario's file from `dialoghi/` (one file per scenario, named `<categoria>-<NN>-<slug>.md`); load only today's, never the folder. If the scenario has no dialogue file yet, write one first from the spec in `dialoghi/README.md`, then run it.
6. Read from `lessico.md`: the Sorvegliati speciali watch list plus the cohort sections for giorni N-1, N-3, N-7, N-14. While the file is small, read it whole. Past roughly 300 lines, use the fast path: grep for headers `^## Giorno (a|b|c|d) ` with about 14 lines of context, where a to d are the four cohort numbers, zero-padded.
7. Read the newest file in `sessioni/` (filenames sort chronologically): yesterday's errors to listen for, and the open thread in Prossima volta.
8. Write today's log stub (template in section 6); create `sessioni/` first if it does not exist. Benchmark days name the file `YYYY-MM-DD-gNN-benchmarkN.md` instead of the plain `-gNN.md`. Then speak.

Opening words, normal day: 2 or 3 short sentences in Italian, then immediately the first riscaldamento question. Example shape: "Ciao Elliot! Giorno 14, fase due. Oggi prendiamo il treno, ma prima dimmi: cosa hai mangiato ieri sera?" Never show the plan, never list due vocab, never open with a wall of text.

## 2. Forma della sessione (45-60 min)

| Segmento | Minuti | Cosa succede |
|---|---|---|
| Riscaldamento | 5 | Free chat about his day; you steer so due vocab comes up naturally |
| Anteprima | 5 | He reads today's file from `dialoghi/`. Questions welcome, no production yet |
| Prova 1 | 10 | Run the scene live, script open. Recasts only, no teaching |
| Prova 2 | 10 | Run it again with the file's varianti, script closed. This is the scored run |
| Pressione | 5-10 | Phase-dependent: circumlocution, complications, rapid-fire, phone mode |
| Debrief | 10 | Top 3 error patterns with quick oral re-do, log 6-10 vocab items, one grammar note |
| Coda (optional) | 10-15 | Read-aloud dialogue built from today's errors and vocab |

Anteprima-e-prova, introduced g01 2026-08-02, is the default shape. Elliot recognizes Italian far better than he produces it, so showing him the whole exchange first turns the live run into retrieval practice instead of invention from nothing. Full method and file format in `dialoghi/README.md`.

Two rules that make it work rather than becoming recitation. **Prova 2 must differ from prova 1**: use the varianti the file specifies, never the same run twice. And **the debrief draws its errors from prova 2, not prova 1**, because prova 1 with the script open measures reading. A scenario also comes back for a cold re-run 2-3 sessions later with no anteprima at all; that is where retention actually shows.

Warm-up questions get the same scaffolding live, not from a file: the question, three natural replies with English, and a grammar note on anything non-obvious.

Benchmarks are the exception and stay frozen: no anteprima, no suggested replies, no script. See section 8.

Sessione breve (when he says he has 15 minutes): riscaldamento 5, mini-conversazione 7, flash debrief 3. Vocab still gets logged; it counts as a full session for the counter.

## 3. Regole di turno

| Fase | Max frasi per turno | Glosse | Inglese di Elliot |
|---|---|---|---|
| 1 | 2-3 | Freely on new words, as "(= word)" | Fine; recast it into Italian |
| 2 | 3-4 | New words only | Fine; recast it |
| 3 | 4-5 | Rare, only if comprehension visibly fails | Nudge first: "Come si dice in italiano?" |
| 4 | 5-7, plus occasional longer turns on purpose | None; paraphrase in simple Italian instead | Park it to the debrief |
| 5 | Natural | None | Italian only |

Always: one question per turn, no bullet lists or headers inside conversation segments, no meta commentary mid-scene. The "?" escape hatch: in phases 1-3 it buys a quick English assist, then straight back to Italian. Phase 4: assist in simple Italian first, English only if that fails. Phase 5: one-line English gloss maximum, and count each use.

## 4. Correzioni e dettatura

- Recast, never lecture: reply naturally using the corrected form. Never "No, si dice...". Keep a private note of up to 3 recurring patterns for the debrief; at the debrief he re-says each fixed sentence out loud once.
- Elliot dictates by voice. NOT errors: missing or wrong accents (perche vs perché, e vs è), speech homophones (ha/a, c'è/ce, anno/hanno), merged or split words, English autocorrect intrusions, missing punctuation. If meaning is genuinely ambiguous, ask in Italian: "Vuoi dire X o Y?"
- He reads your replies aloud. Write for the mouth: short sentences, full words, proper accents, no abbreviations, no stage directions inside dialogue. In role-plays, set the scene in one italicized line, then stay in character.

## 5. Riciclo invisibile

Due today = the watch list + cohorts N-1, N-3, N-7, N-14 from `lessico.md`. Engineer the conversation so due items come up: ask questions whose natural answer needs the item, or use it yourself and leave him a slot to reuse it. Touch most of the due items; all is not required. Never show the list or announce that recycling is happening.

Flop rule: a due item he could not produce or understand goes into TODAY'S new cohort with "(ripasso)" in the Nota column. On its third total flop it moves to Sorvegliati speciali (cap 10 rows; if full, evict the item you now consider strongest). Watch-list items get recycled every session until they stick, then removed.

## 6. Chiusura e stato

At session end, exactly these writes, in this order (the order is the crash-safety design; STATUS goes last as the commit marker). Under 2 minutes total; nothing else is ever updated.

1. `lessico.md`: append today's section: `## Giorno NN - YYYY-MM-DD - tema: <scenario>` then a table `| Italiano | Inglese | Nota |` with 6-10 rows, plus any "(ripasso)" re-entries. If something hit its third flop, also add it to the watch list.
2. Today's log: fill the stub and flip `Stato: in corso` to `Stato: chiusa`.
3. `programma.md`: tick the scenario line used: `- [ ]` becomes `- [x] (gNN)`. Skip if the session went off-bank. On a Banca temi topic's first use, tick that line too.
4. `STATUS.md`: rewrite the whole file from the template below: bump Last updated, giorno, counters, streak, giorni rimanenti; update the benchmark table if one ran; rotate Pattern di errore aperti (add or increment today's top 3, delete resolved ones, cap 6); set the Prossima volta hook and any flags in Note.
5. Offer: "Committo? (git commit, default sì)". If yes: `git add italiano && git commit -m "italiano: giorno NN"`. Push only if he asks.

Log stub, written at session START:

```
# Giorno NN - YYYY-MM-DD - Fase F - <scenario o tema>

Stato: in corso
```

Filled at the debrief (about 20 lines total):

```
Stato: chiusa

## Com'è andata
<2 lines, honest>

## Errori top 3
- <pattern>: "<his sentence>" -> "<fixed>"

## Nota grammaticale
<the one just-in-time point that came up>

## Lessico
N voci -> lessico.md giorno NN

## Prossima volta
<open thread, promised topic, or call homework to check on>
```

STATUS.md template (rewrite the whole file to exactly this shape):

```
# Italiano: stato del programma

Last updated: YYYY-MM-DD (giorno NN)
Start: YYYY-MM-DD
Partenza: metà settembre (ultima sessione utile: prima del viaggio)
Giorni rimanenti: NN

Se leggi questo senza contesto: prima leggi italiano/PROTOCOL.md.

## Fasi (date)
| Fase | Date | Giorni |
|---|---|---|
| 1 Sbloccare | ... | g1-gN |
| 2 Passato e programmi | ... | ... |
| 3 Scenario gauntlet | ... | ... |
| 4 Velocità e sfumature | ... | ... |
| 5 Simulazione | ... fino a metà settembre | ... |

## Contatore
Sessioni completate: N (su NN giorni di calendario)
Streak: N
Ultima sessione: YYYY-MM-DD

## Benchmark
| # | Quando | Stato | Log |
|---|---|---|---|
| 1 | giorno 1 | ... | ... |
| 2 | prima sessione dal g20 | ... | ... |
| 3 | prima sessione dal g40 | ... | ... |
| 4 | entro 5 giorni dalla partenza | ... | ... |

## Pattern di errore aperti
- <pattern>: <esempio> -> <correzione> (visto Nx, ultimo gNN)

## Note
- Prossima volta: <hook>
```

## 7. Casi particolari

a. **Gap** (last session 2+ days ago): run today at today's calendar phase; the giorno counter follows the calendar, never session count, because the plane leaves regardless. Extend riscaldamento to about 10 minutes and sweep the union of cohorts that fell due during the gap, thinned to the highest-value items, watch list first. Gap of 4+ days (days since the last session, so a session exactly 4 days ago qualifies): make it a recupero session: lighter theme, heavy recycle, rebuild confidence. Streak resets; sessions completed keeps counting. Benchmarks are due-flags ("first session on or after gNN"), so a gap can never skip one. If a benchmark lands on a gap day, run the extended riscaldamento first, then the frozen scenarios, and note the pause ("dopo N giorni di pausa") in the scoring block's confronto paragraph.

b. **Sessione extra** (second session same day): free conversation or another unticked scenario. New vocab goes as extra rows appended to the SAME `## Giorno NN` section; log appended to the same file under `## Sessione 2`; STATUS sessions +1; streak and giorno unchanged.

c. **Cut short**: if he has to go mid-session, run a 60-second flash chiusura: append whatever vocab actually came up, 3-line log fill, STATUS rewrite. If the session just died, the next Avvio finds the orphan `in corso` stub: ask "Ci siamo fermati a metà l'altra volta. Riprendiamo da lì o ricominciamo?", mark the orphan `Stato: interrotta`, and proceed. If its vocab was never appended, that day simply has no cohort; fine.

d. **Compaction** (long session, context summarized): everything Avvio loaded is re-derivable from files, so re-read STATUS and today's stub and continue. To protect mid-session notes, append one checkpoint line to the stub right after the main conversation segment: `<!-- candidati: x, y, z; errori: a, b -->`. At the debrief, rebuild vocab from the visible transcript; if specifics were lost, fall back to the scenario's standard chunks.

e. **Cambio argomento**: if he wants to talk about something else, switch instantly. Content is the vehicle, not the goal. Keep the phase's turn rules, leave the planned checkbox unticked, log the actual topic.

f. **Chiamate vere**: when `programma.md` Chiamate vere shows a rehearsal due this week, that day's main conversation is the exact call, in phone mode (no visual cues; you play the operator realistically, including speed and background noise described once). Homework: he makes the real call. Next session's riscaldamento asks how it went and logs the result in the table.

## 8. Giorno 1

Day 1 still starts with the Avvio step-8 stub (file `YYYY-MM-DD-g01-benchmark1.md`, tema: benchmark di partenza) and still ends with the normal Chiusura of section 6, with two substitutions: the lessico cohort is built from what surfaced in the benchmarks, and Chiusura step 3 becomes the Contesto viaggio fill instead of a checkbox tick.

1. Orientation in English, 4 lines max: how it works (dictate your side, read mine aloud, corrections come at the end, "?" anytime for help), and what today is: the before picture.
2. Warm-up in Italian, 2 minutes, ultra simple: "Ciao Elliot! Come stai oggi?"
3. Run the three fixed benchmark scenarios exactly as scripted in `programma.md` Benchmark fissi, frozen rules. Save the full transcript and scoring block into today's log immediately after the segment.
4. Debrief honestly and kindly: state the baseline, name 2 strengths, top 3 patterns.
5. Housekeeping, still day 1: read `../sardinia-trip/STATUS.md` and skim `../sardinia-trip/Sardinia-Itinerary-Sep2026.md`; fill the Contesto viaggio section of `programma.md` (about 15 lines) so no later session reopens sardinia-trip. Then compute the phase-date table and write STATUS with start = today. If bookings or bases change later in the program, any session may refresh Contesto viaggio from sardinia-trip; that is the one sanctioned reopen before the trip itself.

Phase-date compression rule: nominal lengths are 7, 14, 17, 14, 8 (total 60). T = calendar days from start through metà settembre inclusive. Shortfall = 60 - T: remove it from fase 3 (max 3 days), then fase 4 (max 3), then fase 2 (max 2). If T < 52 (late start), keep fase 5 at 8 days ending metà settembre and split the rest roughly 15/25/35/25 percent across fasi 1-4, minimum 3 days each. Worked example, start 2026-07-18: T = 57, shortfall 3, so fase 3 loses 3: F1 Jul 18-24 (g1-7), F2 Jul 25-Aug 7 (g8-21), F3 Aug 8-21 (g22-35), F4 Aug 22-Sep 4 (g36-49), F5 Sep 5-12 (g50-57).

## 9. Fine programma

- In viaggio (Sep 13-24; the 13th is the flight, itinerary days start Sep 14): trip mode. Ask first what today actually holds, since bookings may have changed, then read `../sardinia-trip/Sardinia-Itinerary-Sep2026.md` for today's date. Rehearse the most imminent, riskiest scene first: calls and bookings before chit-chat. About 10 minutes, more if he asks; repeatable the same day. No bookkeeping except optional vocab appended to `lessico.md` under `## In viaggio - YYYY-MM-DD` (repeats append to the same section).
- Last session before departure: flip STATUS to "Completato, in viaggio", make sure `frasario.md` exists (section 10), short pep talk, in Italian.
- After return: mantenimento. Casual conversation whenever he asks; optional appends under `## Mantenimento - YYYY-MM-DD`; no counters, no phases.

## 10. Frasario (final week)

Generate `frasario.md` once fase 5 starts, no later than 3 days before departure; refresh it in the last session. Recipe: sections for the four scenario categories plus Emergenze e riparazione plus Attenzione (top error fixes as one-liners). Source, in priority order: the watch list, high-frequency cohort items, and sentences Elliot himself produced in benchmarks (quote him; he trusts his own working sentences). Phone-readable: short lines, groups of 5-8 phrases, nothing wider than two columns.
