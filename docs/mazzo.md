# Il Mazzo: contratto tra i Dialoghi e l'app

Built for: the listening app (elliotfsl/italiano-app) and `tools/build_mazzi.py`. Decided
metà settembre (ADR-0001). Bilingual Scena/Grammatica/Varianti added metà settembre. Glossary in
`../CONTEXT.md`.

A Mazzo is the JSON the app plays for one Dialogo. It is derived by `tools/build_mazzi.py`
inside the mirror Action and published to the public mirror at `mazzi/<dialogo-slug>.json`,
plus an index at `mazzi/index.json`. Never hand-edited. The build fails, and the mirror is
not updated, if any Dialogo violates the source contract below.

## Source contract (what a Dialogo must contain)

Header line 3, fields separated by ` · `:
`Categoria: <cibo|trasporti|socialita|negozi> · Fase minima: <n> · Banca scenari: "<riga>" · Voce: <maschile|femminile>`

`## Scena`: Italian prose first, then a line containing only `---`, then English prose (the
same scene, in English). Builder emits `sceneIt` and `sceneEn`; `scene` is kept as the
English text, for backward compatibility with the existing app build. A `## Scena` section
missing the `---` separator fails the build.

`## Anteprima`, then one block per turn:

```
### Turno N · SPEAKER: "Battuta in italiano"
*Natural English translation*
*Letteralmente: word for word English, Italian order kept*

| Italiano | English | Nota |
|---|---|---|
| reply 1 | ... | nota italiana / English note |
| reply 2 | ... | ... |
| reply 3 | ... | ... |

**Distrattore.** "wrong reply in Italian" · perché è sbagliata / why it is wrong

**Grammatica.** free prose, may span several paragraphs and tables, until the next ### or ##

**Grammar.** the same content, translated: prose fully in English, table headers and notes
translated, conjugation forms kept in Italian (`finisco`, not "I finish")
```

The English `**Grammar.**` block is a faithful translation of the Italian `**Grammatica.**`
block that precedes it, tables included: translate headers (`Persona` -> `Person`,
`Presente` -> `Present`, `Nota` -> `Note`) and any prose notes, but leave the conjugation
forms themselves in Italian, since those are what the learner has to produce.

Rules the builder enforces:
- exactly three candidate rows per turn; exactly one `*Letteralmente: ...*` line; exactly
  one `**Distrattore.**` line; `Voce:` present in the header
- turns numbered 1..N without gaps
- the Distrattore text must not equal any candidate
- a turn with `**Grammatica.**` but no following `**Grammar.**` block fails the build; a
  turn with neither is fine (both `grammar` and `grammarEn` are empty strings)
- the file's slug must not be one of the frozen benchmark scenarios (the builder carries the
  three Banca scenari lines from `programma.md` "Benchmark fissi" and refuses them)

`## Varianti per la prova 2` is a numbered list. Each item is followed by an indented
italic block giving its English rendering, one line for a short item or several wrapped
lines for a long one: the block opens on a line starting with a single `*` and closes on a
line ending with a single `*` (the same line, for a one-line rendering):

```
1. **I cornetti alla crema sono finiti.** Offro solo semplice o alla marmellata, e lo dico
   dopo che ha già ordinato quello alla crema.
   *The cream croissants are out. I offer only plain or jam, and I say so after he has
   already ordered the cream one.*
```

Builder emits `variants` as `[{"it": ..., "en": ...}]`. A numbered item with no indented
italic English line fails the build.

`## Lessico target` is a table, same shape as a Turno's candidate table:

```
## Lessico target

| Italiano | Inglese | Nota |
|---|---|---|
| il cono | the cone | |
| la coppetta | the cup | maschile e femminile per lo stesso oggetto / masculine and feminine noun for the same object |
```

6 to 10 rows. Nota is bilingual (`it / en`, split on the first ` / ` like every other Nota
cell in the contract) and may be empty. The builder fails, loudly, on the old
comma-separated string form (`## Lessico target\n\nil cono, la coppetta`); that form is no
longer accepted.

## Output schema

```json
{
  "slug": "cibo-02-gelateria",
  "title": "In gelateria",
  "category": "cibo",
  "minPhase": 1,
  "bank": "Gelateria: gusti, coni, coppette, e il gusto che vuoi è terminato",
  "voice": "maschile",
  "scene": "After dinner, ...",
  "sceneIt": "Dopo cena, ...",
  "sceneEn": "After dinner, ...",
  "turns": [
    {
      "n": 1,
      "speaker": "GELATAIO",
      "line": "Buonasera! Coppetta o cono?",
      "natural": "Good evening! Cup or cone?",
      "literal": "Good evening! Little-cup or cone?",
      "candidates": [
        {"it": "Cono, grazie.", "en": "Cone, thanks.", "noteIt": "minima, e la risposta più comune", "noteEn": "minimal, and the most common answer"}
      ],
      "distractor": {"it": "Dammi un cono.", "whyIt": "dà del tu a uno sconosciuto", "whyEn": "uses tu with a stranger"},
      "grammar": "markdown string, Italian, may be empty",
      "grammarEn": "markdown string, English translation of grammar, may be empty"
    }
  ],
  "variants": [
    {"it": "markdown string per numbered item", "en": "English rendering of that item"}
  ],
  "targetVocab": [
    {"it": "il cono", "en": "the cone", "noteIt": "", "noteEn": ""},
    {"it": "la coppetta", "en": "the cup", "noteIt": "maschile e femminile per lo stesso oggetto", "noteEn": "masculine and feminine noun for the same object"}
  ],
  "sourceSha": "<git sha of primatine194 that built it>",
  "builtAt": "metà settembreT00:00:00Z"
}
```

`mazzi/index.json` is `[{"slug","title","category","minPhase","voice","turnCount"}]`.

Notes split on the first ` / ` in the Nota cell; if there is no ` / `, the whole cell goes to
`noteIt` and `noteEn` is empty (the builder warns, does not fail). Same rule for the
Distrattore reason and for the Lessico target table's Nota column. A Lessico target row
with an empty Italiano or Inglese cell fails the build.
