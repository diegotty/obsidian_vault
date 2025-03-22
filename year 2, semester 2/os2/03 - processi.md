---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-22T13:38
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
- **BSS**: (Block Started from Symbol) contiene dati statici non inizializzati 
- **heap**:
- **stack**:
- **memory mapping segment**: