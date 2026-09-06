---
status: accepted
date: metà settembre
---
# One conversation engine, with the Dialogo as a plan, not a script

Elliot found that the app graded an in-scene question ("cosa vuol dire pesche in
inglese?") as a failed reply, because the Lezione treated every reply as an answer to the
current Battuta. Decided: a single engine (the Guida) plays the counterpart for every
mode. The Dialogo's Turni become Tappe, goals the scene must pass through, and the model
decides each exchange whether Elliot answered the current Tappa, took a Deviazione (an
in-world detour or a meta request like a translation), or was not understood. It answers
whatever he said in character, then steers back to the next uncovered Tappa. Guardrails:
stay in the scene, never more than two exchanges away from the plan before steering, keep
the Lei register, answer meta requests in one line (Italian, then English only if he asked
in English), never lecture mid-scene. Scripted mode keeps the Battute verbatim when the
conversation is on plan; Improvvisazione is the same engine with the Varianti as the plan.
Consequence: cards and translations exist for every line, scripted (from the Mazzo) or
generated (from the engine), so rescue and Svelamento never disappear when he goes off
script. Grading is per Tappa, not per exchange: a Deviazione is logged, not scored.
