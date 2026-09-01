# Italiano: specchio pubblico per le sessioni voce

Questo repo è una copia in sola lettura, leggermente anonimizzata, del programma di
italiano conversazionale di Elliot. La fonte di verità è privata; questa copia esiste
perché una chat claude.ai (anche in modalità voce) possa leggerla via URL.

## Se sei Claude e ti hanno mandato qui

1. Leggi `STATUS.md`: dice a che giorno siamo, gli errori aperti, e in "Prossima volta"
   quale dialogo tocca oggi.
2. Leggi `PROTOCOL.md` (il metodo) e il file di oggi in `dialoghi/`.
3. Conduci la sessione seguendo il metodo anteprima-e-prova descritto in
   `dialoghi/README.md`: anteprima, prova 1 col copione, prova 2 con le varianti a
   copione chiuso, pressione, analisi bilingue.
4. Alla fine stampa il post-mortem in UN SOLO blocco di codice (modello in
   `PORTATILI.md`), così Elliot lo incolla nella sessione Claude Code che tiene lo stato.
   Tu non puoi scrivere qui: la scrittura passa sempre da quel paste-back.

Nota: i benchmark congelati si corrono solo nel repo principale, non da questa copia.
Le date del viaggio e i dettagli personali sono volutamente sfumati; non chiederli.

## La riga di avvio che Elliot usa

> Leggi https://raw.githubusercontent.com/elliotfsl/italiano/main/README.md e poi
> STATUS.md e PROTOCOL.md nello stesso repo, poi il dialogo di oggi. Conduci la
> sessione; io passo in modalità voce.
