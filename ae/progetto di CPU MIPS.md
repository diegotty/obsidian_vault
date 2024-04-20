## introduzione
la cpu è una macchina sequenziale, ovvero uno stato + un cirucito combinatorio
![[Pasted image 20240420115353.png]]
il ciclo di clock è il periodo tra due positive edge del clock. esso è la durata dell’istruzione più lenta !!!
## progettare una CPU MIPS
CPU MIPS semplice non ottimizzata (senza pipeline)
- definire come viene eleborata una istruzione
- scegliere le istruzioni da realizzare
- scegliere le unità funzionali  necessarie
- collegare le unità funzionali
- costruire la CU(control unit) che controlla il funzionamento della CPU
- calcolare il massimo tempo di esecuzione delle istruzioni (che ci da il periodo di clock)

### fase di esecuzione di una istruzione

| fase       | descrizione                                                        |
| ---------- | ------------------------------------------------------------------ |
| fetch      | caricamento di una istruzione dalla memoria alla CU                |
| decodifica | decodifica della istruzione e lettura degli argomenti dai registri |
| esecuzione | esecuzione(attivazione delle unità funzionali necessarie)          |
| memoria    | accesso alla memoria                                               |
| write back | scrittura dei risultati nei registri                               |
altre operazioni necessarie:
- aggiornamento del pc (normale/salti /salti non condizionati)

### istruzioni da realizzare per una CPU
- accesso alla me
