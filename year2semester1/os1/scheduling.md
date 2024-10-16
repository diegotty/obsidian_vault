---
created: 2024-10-15
related to: "[[gestione dei processi]]"
updated: 2024-10-15, 09:32
---
lo scheduling gestisce l’allocazione del tempo di esecuzione tra i diversi processi che ne fanno richiesta contemporaneamente
## obiettivi dello scheduling
### long-term scheduling
- decide quali programmi sono ammessi nel sistema per essere eseguiti
- spesso FIFO, o FIFO “corretto” che tiene conto di criteri come la priorità
- controlla il grado di [[introduzione ai sistemi operativi#multiprogrammazione]]
- più processi ci sono, più è piccola la percentuale di tempo per cui ogni processo viene eseguito
tipiche strategie:
- i lavori batch(non interattivi) vengono accodati il LTS li prende quando lo ritiene giusto
- i lavori interattivi vengono ammessi fino a “saturazione” del sistema
- si prova a mantenere un mix tra I/O-bound e CPU-bound jobs
l’LTS può essere chiamato anche quando termina un processo o quando un processo è idle da troppo tempo
### medium-term scheduling
- gestisce lo swapping dei processi (il passaggio da memoria seconda a memoria principale e viceversa)
- gestisce quindi il grado di multiprogrammazione
# short-term scheduling
chiamato anche [[gestione dei processi#dispatcher]], è lo scheduler eseguito più frequentemente
è invocato sulla base di eventi:
- interruzioni di clock
- interruzioni di I/O
- chiamate di sistema
- segnali
lo STS alloca tempo di esecuzione su un processore per ottimizzare il comportamento dell’intero sistema
## criteri per lo STS
bisogna prima effettuare delle distizioni tra i tipi di criteri:
- criteri per utente: minimizzazione del tempo di risposta (tempo tra una richiesta di computazione e la visualizzazione dell’output)
- criteri per sistema: uso efficiente ed effettivo del processore
- criteri correlati alle prestazioni: quantitativi e facili da misurare
- criteri non correlati alle prestazioni: qualitativi, difficili da misurare
### criteri utente
prestazionali:
- **turnaround time**: tempo tra la creazione di un processo e il suo completamento. comprende i vari tempi di attesa (I/O, processore), quindi è un buon criterio per i processi batch, non molto per quelli interattivi
- **response time**: tempo tra la sottomissione di una richiesta e l’inizio della risposta, buono per i processi interattivi in cui si comincia a dare una risposta mentre ancora il processo non è finito
- **deadline**: nei casi in cui si possono specificare scadenze per i processi, lo scheduler dovrebbe come prima cosa massimizzare il numero di scadenze rispettate, con la minor variabiltà nei tempi di risposta/ritorno
non prestazionali:
- **predictability**: non tanta variabilità nella velocità di esecuzione dei processi (se apro una finestra 100 volte e ci mette 1s ogni volta, la 101esima volta non ci dovrebbe mettere 5s)
### criteri di sistema
prestazionali:
- **throughput**: numero di processi completati per unità di tempo (o che abbiano cominciato a produrre output, per gli interattivi)
- **processor utilization**: percentuale di tempo in cui il processore viene utilizzato (il processore deve essere idle il minor tempo possibile)
non prestazionali:
- **fairness**: se non ci sono indicazioni dall’utente o dal sistema, tutti i processi devono essere trattati allo stesso modo: niente favoritismi, niente starvation(che potrebbe succedere se esistono delle priorità)
- **enforcing priorities**: se ci sono priorità, è compito dello scheduler fare in modo che vengano rispettate (es: occorre avere più code, una per ogni livello di priorità)
- **balancing resources**: lo scheduler deve far sì che le risorse del sistema siano utilizzate il più possibile, quindi i processi che useranno meno le risorse attualmente più usate dovranno essere favoriti
>[!figure]  ![[Pasted image 20241014211754.png]]
code per gestione della priorità

### i/o scheduling
//TODO add quando lo facciamo

>[!figure] ![[Pasted image 20241015095636.png]]
>transizioni decise dai vari tipi di scheduler


# politiche di scheduling
>[!figure] ![[Pasted image 20241016081525.png]]
overview delle politiche di scheduling
## funzione di selezione
è quella che sceglie effettivamente il processo da mandare in esecuzione.
se basata sulle caratteristiche dell’esecuzione, i parametri da cui dipende sono:
- w = tempo trascorso in attesa
- e = tempo trascorso in esecuzione
- s = tempo totale richiesto, incluso quello già servito (e)
	- inizialmente e = 0, quindi s va stimato o fornito come input assieme alla richiesta di creazione del processo
## modalità di decisione
specifica quando chiamare la funzione di selezione
**preemptive**:
- se un processo è in esecuzione, allora arrriva o fino a terminazione, o fino ad una richiesta di I/O (o comunque una richiesta bloccante)
**non-preemptive**:
- il SO può interrompere un processo in esecuzione anche senza le condizioni della modalità preemptive, e in questo caso il processo diventerà “ready”
- la preemption di un processo può avvenrire per l’arrivo di nuovi processi (appena forkati) o per un interrupt