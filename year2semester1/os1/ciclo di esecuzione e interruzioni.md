andiamo ad analizzare il ciclo di esecuzione, aggiungendo a mano a mano complessita’
### tipi di istruzioni
- scambio dati tra processore e memoria
- scambio dati tra processore e disp. i/o
- manipolazione di dati 
- controllo (modifica del PC tramite salti condizionati e non)
- operazioni riservate (abilitazione interrupt, abilitazione cache, abilitazione paginazione)
## ciclo a due passi
1. processore legge istruzione dalla memoria principale(fetch) e la carica nell’IR
2. il processore esegue l’istruzione prelevata
3. back to 1.
>[!example] esempio di esecuzione di un programma
>//TODO
# interruzioni
- paradigma dell’interazione hardware/software, interrompono la normale esecuzione sequenziale del processore, che consegue nell’esecuzione del software “di sistema”(parte del sistema operativo), che tipicamente non e’ stato scritto dall’utente che sta eseguendo un certo programma
cause:
- da programma(le uniche sincrone)
- da I/O
- da fallimento hardware
- da timer
## interruzioni sincrone
- hanno luogo come immediata conseguenza di una certa istruzione, al contrario delle interruzioni asincrone che (tipicamente)vengono sollevate molto dopo l’istruzione che le ha causate(se sono state causate da un istruzione to begin with)
le interruzioni di programma possono avere molte cause:
- overflow
- divisione per 0
- debugging
- syscall
- etc…
## interruzioni asincrone
- interruzioni da i/o → generate dal controllore di un dispositivo di i/o, che sono piu’ lenti del processore. quindi il processore manda un comando al dispositivo i/o e poi aspetta che il dispositivo lo “interrompa” quando e’ risucito a completare la richiesta. le interruzioni da i/o segnalano quindi il completamento o l’errore di una operazione i/o
- interruzioni da fallimento hw → cause come: improvvisa mancanza di potenza, errore di parita’ nella memoria
- interruzioni da comunicazione tra CPU → per sistemi con piu’ CPU
- interruzioni da timer → generate da un timer interno al processore

## istruzioni di ritorno
una volta finita l’interruzione, possono accadere diverse cose:
- se l’interruzione era asincrona, se la computazione non e’ stata completamente abortita come conseguenza dell’interruzione, si riprende dall’istruzione subito successiva a quella dove si e’ verificata l’interruzione
- se l’istruzione e’ sincrona, si possono avere:
	- faults(errori corregibili)→ viene rieseguita la stessa istruzione
	- aborts(errori non corregibili)→ si esegue il software collegato con l’errore
	- traps e syscalls → si continua dall’istruzione successiva

## ciclo con controllo per interruzioni
aggiungendo al primo ciclo, dopo ogni fetch-execute, viene controllato se c’e’ stata un’interruzione: se e; cosi, il programma viene sospeso e viene eseguita una funzione che gestisce l’interruzione: una **interrupt-handler routine**
## interrupt handler