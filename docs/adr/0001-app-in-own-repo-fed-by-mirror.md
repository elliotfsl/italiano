---
status: accepted
date: metà settembre
---
# The listening app lives in its own repo and reads Mazzi from the public mirror

The app (elliotfsl/italiano-app, deployed on Railway) never reads the private repo. The
existing mirror Action builds a Mazzo (JSON) from each Dialogo alongside the scrub, fails
the sync if a Dialogo is malformed or lacks its Traduzione letterale, and pushes the Mazzi
into elliotfsl/italiano. The app fetches them from raw URLs. This keeps the scrub as the
single privacy gate, keeps primatine194 out of any deploy pipeline, and means a new Dialogo
becomes playable by pushing markdown. Frozen benchmark scenarios never get a Dialogo, so
they can never get a Mazzo.

Considered: the app inside primatine194 (Railway would need the private catch-all repo and
every unrelated commit would rebuild), and hand-committed Mazzi in the app repo (two places
to update per Dialogo).
