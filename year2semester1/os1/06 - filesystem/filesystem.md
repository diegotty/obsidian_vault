---
created: 2024-11-18
related to: "[[dispositivi IO, buffering]]"
updated: 2024-11-19T10:53
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

una directory è essa stessa un file (speciale), e fornisce il mapping tra nomi dei file e file stessi
le operazioni possibili su una directory sono:
- ricerca
- creazione file
- cancellazione file
- lista del contenuto della directory
- modifica della directory
## schemi per le directory
il metodo usato per memorizzare le informazioni sopra varia da sistema a sistema:
### struttura semplice
lista di entry, una per ogni file
- i file sono memorizzati in modo sequenziale, con il nome dei file da chiave
- in questo modo, 2 file non potranno mai avere lo stesso nome
- non aiuta nell’organizzare i file
### schema a due livelli 
una directory per ogni utente(che al suo interno è solo una lista dei file di quell’utente), più una (master) che le contiene tutte
- la master contiene anche l’indirizzo e le informazioni per il controllo dell’accesso
### schema gerarchico ad albero per le directory
una directory master contiene tutte le altre directory, e ogni directory può contenere un file oppure altre directory utente
- ci sono anche sottodirectory di sistema, sempre dentro la directory master
>[!figure] graph
![[Pasted image 20241119082905.png|350]]

>[!info] directory path
![[Pasted image 20241119083159.png]]
la struttura ad albero permette agli utenti di trovare un file seguendo un percorso nell’albero (**directory path**)
>- è possibile avere nomi duplicati purchè con path diversi, ovvero in directory diverse

solitamente, gli utenti o processi interattivi hanno associata una directory di lavoro (**working directory**), e tutti i nomi dei file sono dati relativamente alla working directory (pensa al terminale !)
# gestione della memoria secondaria
il SO è responsabile dell’assegnamento di blocchi a file, e deve gestire 2 problemi correlati: 
- occorre allocare spazio per i file, e mantenerne traccia una volta allocato
- occorre tenere traccia dello spazio allocabile
chiaramente, un problema influenza l’altro
il SO alloca in “porzioni” o “blocchi”, cioè un sequenza **contigua** di settori
- tipicamente, viene usata la parola **porzioni** quando si parla di allocazione dinamica, e **blocchi** quando si usa la preallocazione
- in caso di porzioni, bisogna decidere se di dimensione fissa o dinamica, e quanto grandi

**allocation table**: per ogni file, mantiene le informazioni su dove sono sul disco le porzioni che lo lo compongono
## allocazione
### preallocazione
- la massima dimensione viene dichiarata a tempo di creazione
- tuttavia, la dimensione è facilmente stimabile in alcune applicazioni, e difficile in molte altre (utenti ed applicazioni sovrastimano la dimensione così da poter effetivamente (e senza problemi) memorizzare le informazioni desiderate nel file)
ciò risulta in uno spreco di spazio su disco
### allocazione dinamica
viene quindi preferita l’allocazione dinamica, in cui la dimensione viene aggiustata in base a syscall come `append` o `truncate`
## dimensione delle porzioni
ci sono due possibilità agli estremi:
- si alloca una porzione larga a sufficienza per l’intero file: in questo modo l’accesso sequenziale è il più veloce
- si alloca un blocco(sequenza di $n$ settori contigui, con $n$ fisso e piccolo (spesso $n = 1$)) alla volta
si cerca un punto d’incontro tra efficienza del singolo file ed efficienza del sistema
- porzioni piccole vuol dire grandi tabelle di allocazione, e quindi grande overhead, ma vuol dire anche maggior facilità di riuso dei blocchi
- le porzioni fisse grandi invece portano a frammentazione interna
- la frammentazione esterna invece è sempre possibile: i file possono essere cancellati (questo riguarda la contiguità delle aree allocate penso, roba della compattazione)
alla fine, ci sono 2 possibilità (valide sia per preallocazione che per allocazione dinamica):
- porzioni grandi e di dimensione variabile
	- ogni singola allocazione è contigua, e la tabella di allocazione è abbastanza contenuta (porzioni grandi)
	- complicata la gestione dello spazio libero !
- porzioni fisse e piccole
	- tipicamente, 1 blocco per porzione
	- molto meno contiguo del precedente
>[!info] preallocazione + porzioni grandi e di dimensione variabile
>se si usa questa combinazione, non c’è bisogno di una tabella di allocazione: per ogni file basta l’inizio e la lunghezza, in quanto ogni file è un’unica porzione
>- funziona come il porzionamento della RAM: si usano algoritmi come bset fit, first fit, next fit
> - rimane inefficiente per lo spazio libera: necessita periodica compattazione, che è molto più onerosa che la compattazione della RAM

## metodi di allocazione
>[!info] riassunto
![[Pasted image 20241119105124.png]]
> - la frequenza di allocazione è un overhead !
### allocazione contigua
un insieme di bl
### allocazione concatenata
### allocazione indicizzata