# Italiano: specchio pubblico per le sessioni voce

Questo repo è una copia in sola lettura, leggermente anonimizzata, del programma di
italiano conversazionale di Elliot. La fonte di verità è privata; questa copia esiste
perché una chat claude.ai (anche in modalità voce) possa leggerla via URL.

## Se sei Claude e ti hanno mandato qui

Questo file è il tuo runbook. Sostituisce i punti di `PROTOCOL.md` che presuppongono di
poter scrivere nel repo. Leggi tutto prima di dire una parola a Elliot.

### Tre fatti su questa modalità

1. **Sei in sola lettura.** Non puoi creare il log in `sessioni/`, non puoi riscrivere
   `STATUS.md`, non puoi aggiungere a `lessico.md`. Quindi: salta il passo 8 dell'Avvio di
   `PROTOCOL.md` (il log stub) e salta per intero la sezione 6, Chiusura. Il tuo UNICO
   output persistente è il post-mortem alla fine. Non dire mai di aver scritto un file.
2. **È una sessione a voce.** Elliot passa in modalità voce subito dopo la riga di avvio.
   Non leggere mai il markdown ad alta voce: niente intestazioni, niente tabelle, niente
   asterischi o backtick, niente nomi di sezione. Parla e basta. Turni corti, 2 o 3 frasi
   e poi ti fermi, una domanda alla volta. Non annunciare il piano e non riassumere i file
   che hai letto: comincia la conversazione.
3. **L'anteprima si legge, non si recita.** Nel metodo anteprima-e-prova Elliot legge il
   file del dialogo da solo, sullo schermo, prima della prova 1. Digli quale file aprire
   (`dialoghi/<nome>.md`, con l'URL raw) e aspetta che dica di aver finito. Non leggerglielo
   tu: sarebbero dieci minuti di tabelle lette a voce.

### Cosa leggere, in quest'ordine, e niente di più

1. `STATUS.md`: giorno, fase, errori aperti, se un **benchmark è dovuto oggi**, e in
   "Prossima volta" quale dialogo tocca.
2. `PROTOCOL.md`, solo le sezioni 2 e 3 (forma della sessione, regole di turno). Il resto
   riguarda la contabilità nel repo e non ti serve.
3. `dialoghi/README.md`: il ciclo anteprima-prova 1-prova 2 e il formato dell'analisi
   bilingue dopo ogni prova.
4. Il file di oggi in `dialoghi/`, quello indicato da STATUS. Solo quello.
5. `programma.md`, sezione "Benchmark fissi", SOLO se STATUS dice che un benchmark è
   dovuto oggi.
6. `PORTATILI.md`, sezione "Il modello di post-mortem": il blocco che stamperai alla fine.
   Leggilo all'inizio, così sai cosa tenere d'occhio durante la sessione.

### Se un benchmark è dovuto oggi

**Viene prima di tutto il resto**, senza riscaldamento lungo, altrimenti la misura è
gonfiata. Regole congelate che sovrascrivono il metodo normale: solo italiano, zero glosse,
nessuna anteprima, nessuna risposta suggerita, battute di apertura e complicazione dette
alla lettera, 4-5 minuti a scenario, i tre scenari nello stesso ordine. Se si blocca, una
sola richiesta di rilancio: "Prego, mi dica pure." Conta in silenzio, per il post-mortem:
compito completato sì/no, quanti "?", quante richieste di ripetere. Poi, se restano tempo ed
energia, il dialogo del giorno. I benchmark si corrono volentieri da qui, perché misurano il
parlato; l'unica condizione è che il blocco punteggi torni dentro il post-mortem.

### Alla fine

Stampa il post-mortem, compilato, in UN SOLO blocco di codice, e digli di incollarlo nella
sua sessione Claude Code. Etichette esatte del modello in `PORTATILI.md`, perché
l'ingestione le cerca per nome. Se un campo non ha contenuto, scrivi `niente`; non
inventare.

Le date del viaggio e i dettagli personali sono volutamente sfumati; non chiederli.

## La riga di avvio che Elliot usa

> Leggi https://raw.githubusercontent.com/elliotfsl/italiano/main/README.md e poi
> STATUS.md e PROTOCOL.md nello stesso repo, poi il dialogo di oggi. Conduci la
> sessione; io passo in modalità voce.
