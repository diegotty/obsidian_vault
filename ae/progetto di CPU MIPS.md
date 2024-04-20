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

| descrizione                    | istruzioni         | tipo   |
| ------------------------------ | ------------------ | ------ |
| accesso alla memoria           | lw, sw             | tipo I |
| salti condizionati             | beq                | tipo I |
| operazioni aritimetico-logiche | add, sub, sll, slt | tipo R |
| salti non condizionati         | j, jal             | tipo J |
| operazioni con costanti        | li, addi, subi     | tipo I |

### unità funzionali necessarie

| unità              | descrizione                                           |
| ------------------ | ----------------------------------------------------- |
| PC                 | registro che contiene l’indirizzo del la l’istruzione |
| memoria istruzioni | contiene le istruzioni                                |
| adder              | per calcolare il PC (successivo o salto)              |
| registri           | contengono argomenti delle istruzioni                 |
| ALU                | fa le operazioni aritmetico-logiche, confronti, etc   |
| memoria dati       | da cui leggere/in cui scriere i dati (load/store)     |
le unità sono collegate da diversi `datapath` (interconnessioni che definiscono il flusso delle informazioni nella CPU)

se un’unità funzionale può ricevere dati da più sorgenti è necessario inserire un `multiplexer(MUX)` per selezionare la sorgente necessaria 

le unità funzionali sono attivate e coordinate dai segnali prodotti dall CU !

# ingredienti
## memoria delle istruzioni, PC, adder