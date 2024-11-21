---
created: 2024-11-18
related to: "[[dispositivi IO, buffering]]"
updated: 2024-11-21T21:41
---
il file system è una delle parti del SO che sono più imporanti per l’utente
proprietà desiderabili
- esistenza a lungo termine
- condivisibilità con altri processi (tramite nomi simbolici)
- strutturabilità (directory gerarchiche)

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
un insieme di blocchi viene allocato per il file quando ques't’ultimo viene creato
- la preallocazione è necessaria, in quanto occorre sapere quanto, al massimo, sarà lungo il file (altrimenti, se crescesse, incontrerebe blocchi già occupati)
- è necessaria una sola entry nella tabella di allocazione dei file: blocco di partenza e lunghezza del file
- ci sarà frammentazione esterna, con conseguente necessità di compattazione
>[!info] immagine di riferimento
![[Pasted image 20241119105708.png]]
>- se mi arrivasse un file lungo 8 blocchi, in teoria ho lo spazio “fisico” per memorizzarlo, ma non in modo contiguo: c’è quindi bisogno di compattare, in questo modo:
![[Pasted image 20241119105831.png]]

### allocazione concatenata
viene allocato un blocco alla volta, e ogni blocco ha un puntatore al prossimo blocco (tipo lista linkata ! la prima parte del blocco sono dati del file, l’ultima (piccola) parte del blocco è il puntatore)
- è necessaria solo una entry nella tabella di allocazione dei file (il blocco di partenza e la lunghezza del file(che è calcolabile cmq))
- niente frammentazione esterna ! 
- è ok per file da accedere sequenzialmente, ma se serve un certo blocco che si trova $b$ blocchi dopo quello inziale, devo scorrere tutta la lista
>[!info] immagine di riferimento
![[Pasted image 20241119110103.png]]
per migliorare l’accesso non sequenziale, si ricorre al **consolidamento**, che consiste nel mettere i blocchi di un file contigui (analogo alla compattazione), in questo modo:
![[Pasted image 20241119110238.png]]

### allocazione indicizzata
via di mezzo tra allocazione contigua e allocazione concatenata !
la tabella di allocazione contiene una sola entry, con l’indirizzo di un blocco: questo blocco, in realtà, ha una enty per ogni porzione allocata al file (guarda foto). quindi funziona da tabella, anche se si trova in un blocco apparentemente indistinguibile dagli altri
-  perchè ciò sia possibile, ci deve essere un bit che indica se un blocco è composto da dati o è un indice
- se il file è troppo grande, si creano più livelli

>[!info] allocazione indicizzata con porzioni di lunghezza fissa
![[Pasted image 20241121211238.png]]
> l’allocazione con blocchi di lunghezza fissa evita la frammentazione esterna, in quanto \\QUESTION
>- a volte occore il consolidamento, per migliorare la località

>[!allocazione indicizzata con porzioni di lunghezza variabile]
![[Pasted image 20241121211723.png]]
l’allocazione con blocchi di lunghezza variabile migliora la località ! in quanto i blocchi allocati sono contigui
il blocco che fa da tabella ora contiene anche la lunghezza (il numero di blocchi contigui allocati a partire dallo start block)
>- a volte occorre il consolidamento, per ridurre le dimensioni della tabella
>- la tabella è ordinata !
# gestione dello spazio libero
la gestione dello spazio libero è altrettando importante di quello occupato, e per allocare spazio per i file, è necessario sapere qual è lo spazio libero ! (non è realistico guardare la tabella di allocazione di tutti i file per determinare quali blocchi/porzioni sono liberi)
- serve quindi una **tabella di allocazione di disco**, oltre a quella di allocazione per i file, che viene aggiornata si alloca o si cancella un file
esistono diversi modi per gestire la gestione dello spazio libero: 
## tabelle di bit
si usa un vettore con un bit per ogni blocco su disco: 0 vuol dire libero, 1 vuol dire occupato
- questa soluzione minimizza lo spazio richiesto alla tabella di allocazione del disco
ha però dei problemi: 
- se il disco è quasi pieno, la ricerca di uno spazio libero può richiedere molto tempo (risolvibile con delle tabelle riassuntive di porzioni della tabella di bit)
## porzioni libere concatenate
come allocazione libera: le porzioni libere possono essere concatenate le une alle latre, usando, per ogni blocco libero, un puntatore ed un intero per la dimensione
- questa soluzione non ha praticamente overhead
ha però dei problemi:
- occorre leggere un blocco libero per conoscere il prossimo: se occorre allocarne molti, diventa time consuming 
- è lungo anche cancellare file molto frammentati (xke poi devo aggiungerli alla linked list)
## indicizzazione
si tratta lo spazio libero come un file, e quindi si usa un indice come si farebbe per un file