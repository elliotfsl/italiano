# Il Mazzo: contratto tra i Dialoghi e l'app

Built for: the listening app (elliotfsl/italiano-app) and `tools/build_mazzi.py`. Decided
metà settembre (ADR-0001). Glossary in `../CONTEXT.md`.

A Mazzo is the JSON the app plays for one Dialogo. It is derived by `tools/build_mazzi.py`
inside the mirror Action and published to the public mirror at `mazzi/<dialogo-slug>.json`,
plus an index at `mazzi/index.json`. Never hand-edited. The build fails, and the mirror is
not updated, if any Dialogo violates the source contract below.

## Source contract (what a Dialogo must contain)

Header line 3, fields separated by ` · `:
`Categoria: <cibo|trasporti|socialita|negozi> · Fase minima: <n> · Banca scenari: "<riga>" · Voce: <maschile|femminile>`

`## Scena`: free English prose. Whole section becomes `scene`.

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
```

Rules the builder enforces:
- exactly three candidate rows per turn; exactly one `*Letteralmente: ...*` line; exactly
  one `**Distrattore.**` line; `Voce:` present in the header
- turns numbered 1..N without gaps
- the Distrattore text must not equal any candidate
- the file's slug must not be one of the frozen benchmark scenarios (the builder carries the
  three Banca scenari lines from `programma.md` "Benchmark fissi" and refuses them)

`## Varianti per la prova 2` and `## Lessico target` are carried through verbatim.

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
      "grammar": "markdown string, may be empty"
    }
  ],
  "variants": ["markdown string per numbered item"],
  "targetVocab": ["il cono", "la coppetta"],
  "sourceSha": "<git sha of primatine194 that built it>",
  "builtAt": "metà settembreT00:00:00Z"
}
```

`mazzi/index.json` is `[{"slug","title","category","minPhase","voice","turnCount"}]`.

Notes split on the first ` / ` in the Nota cell; if there is no ` / `, the whole cell goes to
`noteIt` and `noteEn` is empty (the builder warns, does not fail). Same rule for the
Distrattore reason.
