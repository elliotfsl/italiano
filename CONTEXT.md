# Italiano (conversation trainer)

Elliot's Italian conversation program: a markdown corpus (protocol, status, dialogues,
vocab) and the tools that run sessions from it, including a private listening app.

## Language

**Dialogo**:
One scenario written as a markdown file in `dialoghi/`, turn by turn, with candidate
replies and glosses. The canonical, human-authored source of lesson content.
_Avoid_: script, lesson file, scene file

**Mazzo**:
The machine-readable form of one Dialogo that the app plays. Derived, never hand-edited.
_Avoid_: deck (in docs), JSON, lesson

**Turno**:
One exchange inside a Dialogo: the other speaker's line plus Elliot's reply.
_Avoid_: step, exchange, prompt

**Battuta**:
The other speaker's line in a Turno, the thing Elliot hears.
_Avoid_: prompt, cue, question

**Risposta candidata**:
One of the three structurally different replies a Dialogo offers for a Turno, each with a
register note.
_Avoid_: option, answer key, suggestion

**Traduzione letterale**:
Word-for-word English rendering of a Battuta, order and idiom preserved, for training the
ear. Distinct from the natural translation.
_Avoid_: gloss, transliteration

**Traduzione naturale**:
The English a native speaker would say for the same Battuta.
_Avoid_: translation (unqualified), meaning

**Benchmark fisso**:
One of the three frozen unassisted scenarios used to measure progress. Never gets a
Dialogo, a Mazzo, or any scaffolding.
_Avoid_: test, exam

**Distrattore**:
The fourth card in a Turno: a plausible wrong reply built from one of Elliot's open error
patterns. Choosing it is graded wrong with the reason.
_Avoid_: trap, wrong answer, filler

**Soccorso**:
Any help Elliot reaches for mid Turno: choosing cards instead of speaking, a reveal, or a
replay. Counted, never forbidden.
_Avoid_: hint, cheat, help

**Svelamento**:
Showing what was only heard, in fixed layers: Italian text, then Traduzione letterale, then
Traduzione naturale.
_Avoid_: unhide, show text, subtitles

**Tentativo**:
One recorded response to one Turno: the mode used (spoken or card), any Soccorso, the
transcript or card chosen, and the grade. The unit the app stores.
_Avoid_: attempt log, result, event

**Post-mortem**:
The single block the app prints at session end, in the canonical template from
`PORTATILI.md`, that Elliot pastes into a repo session. The only bridge from app to repo.
_Avoid_: report, summary, export

**Lezione**:
One pass through one Dialogo in the app, Turno by Turno, with the response mode chosen per
Turno. Repeatable.
_Avoid_: session (that word means a repo or voice session), run, drill

**Ponte**:
The one short in-character line the other speaker says in reaction to Elliot's spoken reply,
before the next Battuta. Either acknowledges, or says it did not understand.
_Avoid_: bridge, follow-up, filler line

**Ripasso a freddo**:
A Lezione on a Dialogo already passed at least twice, with cards off and Svelamento allowed
only after answering. The retention measure.
_Avoid_: test mode, hard mode, review

**Alternativa**:
A Risposta candidata Elliot did not use, shown after a correct reply and again in the
Post-mortem.
_Avoid_: alternate phrase, other option

**Anteprima**:
The read-and-listen screen before a Lezione: scene, role, and every Turno with its
translations, Risposte candidate, and grammar note. Skippable, and never shown before a
Ripasso a freddo.
_Avoid_: preview, briefing, study mode

**Improvvisazione**:
A Lezione where the other speaker is played live from the Dialogo's scene and Varianti,
with no fixed Battute, no cards, spoken replies only. The app's prova 2.
_Avoid_: free mode, live mode, prova 2 (in the app UI)

**Ripasso andato male**:
A Turno that was answered correctly in an earlier pass of the same Dialogo and needed a
Soccorso or was wrong in the latest pass; also a target word never produced across passes.
_Avoid_: regression, forgotten, failed review

**Guida**:
The engine that plays the counterpart in every mode: it reads the plan, hears Elliot,
answers in character, and steers the scene back toward the next Tappa.
_Avoid_: bot, agent, script runner

**Tappa**:
One goal the scene must pass through, taken from a Dialogo's Turni (scripted mode) or its
Varianti (Improvvisazione). Passing a Tappa is what gets graded.
_Avoid_: step, checkpoint, turn (that is a Turno)

**Deviazione**:
An exchange that does not answer the current Tappa: an in-world detour (asking a price,
changing the order) or a meta request (a translation, a repeat, "come si dice"). Answered
in character, logged as a Soccorso when it was a meta request, never graded.
_Avoid_: off-script, error, interruption

**Verdetto**:
The one-word outcome shown after a graded Tappa: Bene, Quasi, or Riprova, with its color.
_Avoid_: score band, result, grade label
