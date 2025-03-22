---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-22T14:09
completed: false
---
# processi
in linux, le due entità fondamentali sono:
- **file**: descrivono/rappresentano le risorse
- **processi**: permettono di elaborare dati e usare le risorse. un file eseguibile, in esecuzione, è un **processo**
>[!example] esempi di processi
>esempi di processo sono quelli creati eseguendo i comandi delle lezioni precedenti: `dd, ls, cat, cp, ln, ....`
>ma non tutti i comandi creano dei processi: ad esempio `echo` o `cd` vengono eseguiti all’interno del processo di shell

>[!info] ridirezione dell’output
i simboli < e > possono essere utilizzati per redirigere l’output di un comando su un file ! cool !

## rappresentazione dei processi
[[gestione dei processi#elementi di un processo|roba già fatta a os1 ngl]]
### PCB
Il PCB è unico per ogni processo e contiene:
- PID: Process Identifier
- PPID: Parent Process Identifier
- real UID: Real User Identifier
- real GID: Real Group ID
- effective UID: Effective User Identifier (UID assunto dal processo in esecuzione)
- effective GID: Effective Group ID (come sopra per GID)
- saved UID: Saved User Identifier (UID avuto prima dell’esecuzione del SetUID)
- saved GID: Saved Group Identifier (come sopra per GID)
- current Working Directory: directory di lavoro corrente
- umask: file mode creation mask
- nice: priorita statica del processo
### aree di memoria
- **text segment**: le istruzioni da eseguire (in linguaggio macchina)
- **data segment**: dati statici inizializzati (variabili globali e locali static)e alcune env vars
- **BSS**: (Block Started from Symbol) contiene dati statici non inizializzati. la distinzione dal data segment si fa per motivi di realizzazione hardware (ok bro ….. un po vago bro …)
- **heap**: dati dinamici
- **stack**: chiamate a funzioni con corrispondenti dati dinamici
- **memory mapping segment**: tutto ciò che riguarda librerie esterne dinamiche usate dal processo, nonchè estensione dello heap in alcuni casi
>[!figure] illustrazione
![[Pasted image 20250322134008.png]]
## stato di un processo
- **running (R)**: in esecuzione su un processore
- **runnable (R)**: pronto per essere eseguito; aspetta scheduler
- **(interruptible) sleep (S)**: in attesa di qualche evento; non può essere scelto dallo scheduler
- **zombie (Z)**: terminato, il suo PCB viene ancora mantenuto dal kernel perchè il processo padre non ha ancora richiesto il suo “exit status”
- **stopped (T)**: caso particolare di sleeping: ha ricevuto un segnale `STOP` ed è in attesa di un segnale `CONT`
- **traced (t)**: in esecuzione di debug, oppure in generale in attesa di un segnale
- **uninterruptible sleep (D)**: come sleep, ma tipicamente sta facendo operazioni di IO su dischi lenti e non può essere interrotto ne ucciso
## modalità di esecuzione dei processi
- **foreground**: il comando può leggere l’input da tastiera e scrivere su schermo, e finchè non termina, il prompt non viene restituito e non si possono **sottomettere** altri comandi alla shell
- **background**: il comando non può leggere l’input da tastiera ma può scrivere su schermo. il prompt viene immediatamente restituito dopo aver eseguito il comando (non aspetta che il processo finisca) e si possono quindi dare altri comandi alla shell da subito
l’esecuzione in background è possibile con il carattere `&`
- non disponibile in tutte le shell, ma in bash si
>[!] `bg`e`fg`
>- `bg` porta un processo in background
>- `fg%n` dove `%n` è il numero del job, lo riporta in foreground
>- `bg%n` dove `%n` è il numero del job, lo porta in background

## pipelining dei comandi
è possibile eseguire un job composto da più comandi:
```
comando1 | comando2 | .... comando n
```
dove lo standard output di un comando $i$ diventa l’input del comando $i+1$
- se uso `|&`, viene ridirezionato lo standard error invece dello standard output allo standard input del comando successivo !
## comandi
### $\verb |ps [opzioni] [pid ...]|$
mostra le informazioni dei process in esecuzione, e legge le informazioni dai file virtuali in `/proc`
- `ps` senza argomenti mostra i processi dell’utente attuale lanciati dalla shell corrente ! (x ogni processo mostra PID, TTY, TIME e CMD)
- `[-e]`: tutti i figli del processo 0, cioè tutti i processi di tutti gli utenti lanciati da tutte le shell o al boot
- `[-u] {utente}`: tutti i processi degli utenti nella lista
- `[-p] {pid, ..}`: tutti i processi con i PID nella lista
- `[-f]`: full-format listing
- `[-l]`: display BSD long format
- `[-o] {field, ...}`: user defind format, per scegliere i campi da visualizzare 
### $\verb |top [-b] [-n \nu m] [-p {pid,}]|$
`ps` interattivo, dinamico e real-time
- `[-b]`: non accetta più comandi interattivi, ma continua a fare refresh ogni pochi secondi
- `[-n num]`: fa solo `num` refresh