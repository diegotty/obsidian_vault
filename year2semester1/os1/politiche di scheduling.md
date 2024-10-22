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
- dopo un completamento di I/O, il processo non va in coda ai ready, ma va in una nuova coda che ha priorità su quella dei ready
- quando un processo viene scelto dalla nuova coda, però, va in esecuzione solo per la porzione di quanto che ancora gli rimaneva da completare
in questo modo, viene migliorata la fairness del round-robin semplice
>[!figure]
![[Pasted image 20241022080958.png]]

# SPN
la politica di scheduling SPN gestisce l’allocazione del tempo di esecuzione in questo modo: **il prossimo processo da mandare in esecuzione è quello più breve**
(con breve, si intende il processo ready in cui il tempo di esecuzione stimato è minore)
- in questo modo, i processi corti scavalcano quelli lunghi
- se il tempo di esecuzione stimato si rivela inesatto, il sistema operativo può abortire il processo
l’SPN  è non-preemptive
## problemi dell’SPN
- la predicibilità dei processi lunghi è ridotta
- i processi lunghi potrebbero soffrire di starvation
## stima del tempo di esecuzione
se il processo non nasce fornendo una stima del tempo di esecuzione stimato, si può effettuare una stima
dato che alcuni processi sono eseguiti varie volte, posso usare il passato ($T_{i}$), per predire il futuro($S_{i}$)
intuitivamente, si potrebbe usare la formula:
$$S_{n+1}=\frac{1}{n}\sum_{i=1}^{n}T_{i}$$

che può essere ottimizzata: la seguente formula ottiene lo stesso risultato, ma ricordando solo l’ultima stima e il tempo di esecuzione dell’ultima istanza del processo(e con molti meno calcoli):
$$S_{n+1}=\frac{1}{n}T_{n}+\frac{n-1}{n}S_{n}$$
ma la stima sopra può essere ancora ottimizzata, cambiando approccio:
si usa l’**exponential averaging**, cioè si fa pesare di più le istanze più recenti

la formula sopra può essere riscritta in questo modo:
$$S_{n+1}=\alpha T_{n}+(1-\alpha)S_{n}, 0<\alpha<1$$

sviluppando la formula(sostituendo $S_{n}$ con la formula stessa), si ottiene:
$$S_{n+1}=\alpha T_{n}+\dots \alpha(1-\alpha)^i\,\,T_{n-i}+\dots+(1-\alpha)^n\,S_{1}$$
dato che $(1-\alpha$) viene elevato ogni volta ad un esponente più grande, $