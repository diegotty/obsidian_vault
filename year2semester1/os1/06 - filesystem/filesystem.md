---
created: 2024-11-18
related to: 
updated: 2024-11-19T08:19
---
il file system è una delle parti del SO che sono più imporanti per l’utente
proprietà desiderabili
- esistenza a lungo termine
- condivisibilità con altri processi (tramite nomi simbolici)
- strutturabilità (directory gerarchice)

i file sono gestiti da un insieme di programmi e librerie di utilità. tali programmi costituiscono il **File(Management)System**, e vengono eseguiti come processi privilegiati (**in kernel mode**)
- le librerie vengono invece invocate come syscall (quindi comunque in kernel mode)
- tali programmi e librerie forniscono un’astrazione sotto forma di operazioni tipiche(creazione, cancellazione, apertura, lettura, scrittura, chiusura) , e mantengono dei dati sui file (metadati: proprietario, data di creazione, etc…)

## terminologia
**campi**: dati di base, valori singoli caratterizzati da una lunghezza e un tipo di dato (es: carattere ASCII)
**record**: insiemi di campi correlati. ogni record è trattato come un’unità (es: un impiegato è caratterizzato dal record nome, cognome, matricola, stipendio)
**file**: insiemi di record correlati (nei sistem moderni, ogni record è un solo campo con un byte). ogni file viene trattato come un’unità con nome proprio, e possono implementare meccanismi di controllo dell’accesso(alcuni utenti possono accedere ad alcuni file, altri no)
**database**: contengono collezioni di dati, e mantengono anche relazioni tra gli elementi memorizzati. i DBMS sono processi di un SO (idk wtf he’s saying ngl)
# sistemi per la gestione di file
## obiettivi
- rispondere alle necessità degli utenti riguardo alla gestione dei dati (creazione, spostamento, cancellazione, etc…)
- garantire che i dati nei file siano validi
- ottimizzare le prestazioni (throughput e tempo di risposta)
- fornire supporto per diversi tipi di memoria secondaria (dischi magnetici, chiavi USB, CD, etc…)
- minimizzare i dati persi o distrutti
- fornire interfacce standard per i processi utente
- fornire supporto per l’I/O effettuato da più utenti in contemporanea
## requisiti
1. ogni utente dev’essere in grado di creare, cancellare, leggere e modificare un file
2. ogni utente deve poter accedere, in modo controllato, ai file di un altro utente
3. ogni utente deve poter leggere e modificare i permessi di accesso ai propri file
4. ogni utente deve poter ristrutturare i propri file in modo attinente al problema affrontato
5. ogni utente deve poter muovere dati da un file ad un altro
6. ogni utente deve poter mantenere una copia di backup dei propri file
7. ogni utente deve poter accedere ai propri file tramite nomi simbolici
# directory
le directory contengono dei file e i loro metadati:
- nome del file (nome scelto dall’utente), che deve essere unico in una directory data
- tipo del file (eseguibile, binario, etc..)
- organizzazione del file (per i sistemi che supportano diverse organizzazioni)
- volume ( il dispositivo su cui il file è memorizzato)
- indirizzo di partenza ( da quale settore e traccia di disco)
- dimensione attuale, dimensione allocata (dimensione massima del file)
- proprietario ( che può concedere/negare premessi ad altri utenti, e può anche cambiare tali impostazioni)
- informazioni sull’accesso ((un file o una directory ?)potrebbe contenere username e password per ogni utente autorizzato)
- azioni permesse (lettura, scrittura, esecuzione, etc..)
- data di creazione, identità del creatore
- data dell’ultimo accesso in lettura e scrittura
- identità dell’ultimo lettore e scrittore
- data dell’ultimo backup
- uso attuale


- una directory è essa stessa un file (speciale), e fornisce il mapping tra nomi dei file e file stessi
le operazioni possibili su una directory sono:
- ricerca
- creazione file
- cancellazione file
- lista del contenuto della directory
- modifica della directory