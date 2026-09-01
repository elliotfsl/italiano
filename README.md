# Italiano: specchio pubblico per le sessioni voce

Questo repo è una copia in sola lettura, leggermente anonimizzata, del programma di
italiano conversazionale di Elliot. La fonte di verità è privata; questa copia esiste
perché una chat claude.ai (anche in modalità voce) possa leggerla via URL.

## Se sei Claude e ti hanno mandato qui

1. Leggi `STATUS.md`: dice a che giorno siamo, gli errori aperti, se c'è un **benchmark
   dovuto oggi**, e in "Prossima volta" quale dialogo tocca.
2. **Se STATUS dice che un benchmark è dovuto, quello viene prima di tutto il resto.**
   Copioni e regole congelate in `programma.md`, sezione "Benchmark fissi". Regole che
   sovrascrivono il metodo normale: solo italiano, zero glosse, nessuna anteprima, nessuna
   risposta suggerita, battute di apertura e complicazione dette alla lettera, 4-5 minuti a
   scenario, i tre scenari nello stesso ordine. Se si blocca, una sola richiesta di
   rilancio: "Prego, mi dica pure." Alla fine compila il blocco punteggi di `programma.md`
   e mettilo nel post-mortem. Poi, se resta tempo ed energia, passa al dialogo del giorno.
3. Leggi `PROTOCOL.md` (il metodo) e il file di oggi in `dialoghi/`.
4. Conduci la sessione seguendo il metodo anteprima-e-prova descritto in
   `dialoghi/README.md`: anteprima, prova 1 col copione, prova 2 con le varianti a
   copione chiuso, pressione, analisi bilingue.
5. Alla fine stampa il post-mortem in UN SOLO blocco di codice (modello in
   `PORTATILI.md`), così Elliot lo incolla nella sessione Claude Code che tiene lo stato.
   Tu non puoi scrivere qui: la scrittura passa sempre da quel paste-back.

Nota: i benchmark si corrono volentieri da qui, perché misurano il parlato e questa è la
copia che serve le sessioni voce. L'unica condizione è che il blocco punteggi torni dentro
il post-mortem, altrimenti la misura è persa.

Le date del viaggio e i dettagli personali sono volutamente sfumati; non chiederli.

## La riga di avvio che Elliot usa

> Leggi https://raw.githubusercontent.com/elliotfsl/italiano/main/README.md e poi
> STATUS.md e PROTOCOL.md nello stesso repo, poi il dialogo di oggi. Conduci la
> sessione; io passo in modalità voce.
