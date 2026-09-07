# PACK-SPEC: pacchetto v2, assembled by build_voice_pack.py

One paste, one message, plain UTF-8 text, LF line endings, no BOM. Section separators are
lines of the form `=== NAME ===`, never markdown headers, so a voice model has nothing to
read aloud as a heading. No em dashes and no en dashes anywhere in generator-written text;
the Dialogo's own `·` in its header lines stays.

Invocation: `python italiano/tools/build_voice_pack.py --dialogo <slug> [--dialogo <slug> ...] [--benchmark]`.
Pass one `--dialogo` per scene STATUS names for today, in STATUS's order. A cold review
(`ripasso a freddo`) is one of those scenes and is flagged in the STATUS excerpt, not by a
separate switch.

## Assembly order

1. **Core prompt**, verbatim from `voce/PROMPT.md`, ending with `Begin now, in character, in Italian.`
2. Blank line, then **`voce/MODE-CHATGPT.md`**, verbatim.
3. `=== STATUS (estratto) ===` then one field per line, computed from the private
   `D:\cc proj\primatine194\italiano\STATUS.md` at generation time:
   - `Start: YYYY-MM-DD` (the Start line, verbatim; rule 7.6 recomputes Giorno from it)
   - `Data: YYYY-MM-DD` (the planned session date)
   - `Giorno: NN` (calendar days from Start through Data, start day 1, per PROTOCOL 7a the
     counter follows the calendar and never the session count)
   - `Fase: <phase the STATUS date table gives for Data>` plus ` (da ricalibrare)` for as
     long as STATUS carries its stale-table warning
   - `Benchmark dovuto oggi: sì, benchmark 1 (tardivo, gNN), copre anche la finestra del n. 2`
     or `no`
   - `Pattern di errore aperti:` then the STATUS bullet list, verbatim, one per line
   - `Scene di oggi:` then one line per scene, in order, each `<slug>` plus `(nuova)` or
     `(ripasso a freddo)`. On a benchmark day these run after the benchmark.
   - `Prossima volta (da STATUS):` the note, verbatim
   - `Sessioni completate: N` and `Ultima sessione: YYYY-MM-DD`
4. **Only if a benchmark is due:** `=== BENCHMARK FISSI ===` then, for each of the three
   scenarios in order from `programma.md` section "Benchmark fissi": the `### Scenario N: ...`
   title line, the `Scene:` line, and the Opener / Mid complication / Closer lines verbatim,
   quotes intact, `...` pauses intact. Drop the scoring block and the definitions paragraph;
   they arrive in part 6. Apply the home-city scrub (see below), so Scenario 3's closer reads
   `E Lei, mi consiglia qualcosa della sua città?`
5. For each scene in STATUS's order, `=== SCENA: <slug> ===` (a cold review gets
   `=== SCENA (RIPASSO A FREDDO): <slug> ===`) then the Dialogo, trimmed.
   - **Include verbatim:** the `# ...` title line; the `Categoria · Fase minima · Banca
     scenari · Voce` header line; the whole `## Scena` section, both halves and its `---`
     separator; per Turno, the `### Turno N · RUOLO: ...` heading, the italic natural
     translation line, the three-row Risposte table (Italiano | English | Nota), and the
     `**Distrattore.**` line; the whole `## Varianti per la prova 2` section including the
     indented italic English lines; the whole `## Lessico target` table.
   - **Drop:** every `*Letteralmente: ...*` line; every `**Grammatica.**` and `**Grammar.**`
     block; every `**Coniugazione:**` / `**Conjugation:**` table and the prose around it.
   - **A cold review scene drops more:** no Risposte tables and no Distrattore lines, because
     core rule 2.7 forbids candidate replies in a ripasso a freddo. Keep the Scena, the Turno
     headings with their natural translations, the Varianti and the Lessico target.
   - Why: grammar prose is roughly 60 percent of a Dialogo and the Guida is forbidden to
     lecture mid-scene. The Risposte and the Distrattore feed rule 4.1, the Varianti feed
     rule 2.6, Lessico target lets the debrief cross-check LESSICO NUOVO, and the Scena is
     what Elliot reads if he says "anteprima".
6. `=== MODELLO POST-MORTEM ===` then the template from `PORTATILI.md` section "Il modello
   di post-mortem", verbatim inside its code fence, followed by the scoring definitions
   paragraph directly under it (Compito, Riparazioni, Comprensione, Scioltezza, Correttezza,
   Lessico). Labels untouched, byte for byte.
7. Final line: `=== FINE PACCHETTO === Comincia adesso, in italiano, in scena.`

## Scrub rule

The pack is pasted into a third-party app, so it gets the same treatment as the public
mirror. Before writing the file, run the pack text through the same replacement list and the
same denylist as `italiano/tools/scrub_mirror.py`, importing them rather than restating them.
A denylist hit fails the build and writes no file. That covers the exact travel dates and the
home city, which are the two things that must never leave the private repo.

One correction the mirror script still needs, and the pack must apply either way: the generic
`della mia città -> della mia città` rule turns Benchmark Scenario 3's closer into a
Sardinian local asking about his own city, which is nonsense and changes what the benchmark
measures. Add the specific pair `mi consiglia qualcosa della sua città -> mi consiglia
qualcosa della sua città` ahead of the generic one, in `scrub_mirror.py` and in the pack
builder.

## Character budget

Core about 8,800, mode block about 1,000, STATUS excerpt about 900, benchmark block about
1,700 when due, each trimmed Dialogo about 5,000 to 6,500, template with definitions about
1,900, plus separators. Target under 19,000 on a one-scene day and under 24,000 on a
benchmark day with a cold review; hard cap 28,000. When a trimmed Dialogo runs past 7,000,
drop the italic English lines under its Varianti first, then the English half of its Scena.
Never drop the Risposte tables or the Distrattore lines from a scene that is not a cold
review.

## Generator checks before writing the file

- Parts 1, 2, 3, 4 and 7 contain zero em dash characters (U+2014) and zero en dash characters (U+2013).
- Part 6 carries the sub-labels `Citazioni (2 o 3 sue frasi, verbatim):`,
  `Giudizio complessivo (un paragrafo, onesto):` and
  `Confronto col benchmark precedente (salta al run 1):`, and `COM'È ANDATA` with its accent.
- Every `--dialogo` slug resolved to a real file in `dialoghi/`.
- The scrub denylist returned no hits.
- Total length under the cap.

Save as `pacchetti/gNN-v2.md`. The pack is disposable once its post-mortem is ingested.
