---
created: 2024-10-16
related to: "[[intro allo scheduling]]"
updated: 2024-10-16, 08:28
---
//TODO index
useremo uno scenario comune di esempio per valutare le diverse politiche di scheduling:
>[!figure] ![[Pasted image 20241014212633.png]]

# FCFS
**first come, first serve** :)
tutti i processi sono aggiunti alla coda dei processi ready, e quando un processo smette di essere eseguito, si passa la processo che ha aspettato di più nella coda ready finora
- non preemptive
>[!figure] ![[Pasted image 20241016083505.png]]
>visualizzazione della politica FCFS
## downsides:
- un processo corto dovrebbe poter aspettare molto prima di essere eseguito
- favorisce i processi CPU-bound, che una volta che si prendono la CPU, non la rilasciano più finchè non hanno finito
# round-robin
anche chiamato **time slicing**, si basa un un clock per usare la preemption e quindi ogni processo ha una fetta di tempo
>[!figure] ![[Pasted image 20241016083813.png]]
round robin con slice = 1 unità di tempo

viene quindi generata un’interruzione di clock ad intervalli periodici, e quando l’interruzione arriva, il processo attualmente in esecuzione viene rimesso nella coda dei ready, e il processo seguente viene selezionato
>[!example] 
![[Pasted image 20241016085037.png]]

## misura del quanto di tempo per la preemption
>[!figure]
![[Pasted image 20241022080229.png]]

per essere ottimale, il quanto di tempo deve essere poco più lungo del “tipico” tempo di interazione per un processo, ma non troppo lungo, altrimenti si degenera in FCFS
## round-robin virtuale
con il round-robin, i processi CPU-bound sono favoriti, in quanto usano tutto il loro quanto di tempo, mentre gli I/O bound ne usano solo una porzione (fino alla richiesta di I/O)
- ciò, oltre ad essere non equo, aumenta la variabilità della risposta e quindi diminuisce la predicibilità
il **round-robin virtuale** mira a risolvere questi problemi:
- dopo un completamento di I/O, il processo non va in coda ai ready,