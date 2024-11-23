---
created: 2024-11-18
related to: "[[dispositivi IO, buffering]]"
updated: 2024-11-23T13:03
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

>[!info] ora possiamo capire meglio cosa vuol dire formattare un file system !
vuol dire prepararlo per raccogliere le informazioni secondo la struttura di un certo file system
es: se un file system usa l’allocazione indicizzata, è necessario che ci sia una file allocation table (vuota)
# gestione dello spazio libero
la gestione dello spazio libero è altrettando importante di quello occupato, e per allocare spazio per i file, è necessario sapere qual è lo spazio libero ! (non è realistico guardare la tabella di allocazione di tutti i file per determinare quali blocchi/porzioni sono liberi)
- serve quindi una **tabella di allocazione di disco**, oltre a quella di allocazione per i file, che viene aggiornata si alloca o si cancella un file
esistono diversi modi per gestire la gestione dello spazio libero: 
>[!warning] una parte della tabella che indica lo spazio libero va sempre tenuta in RAM
>spesso le operazioni di modifica sulla tabella dello spazio libero (quando un file viene allocato\deallocato) vengono registrate solo in memoria principale, e poi in seguito riportate su disco. ciò può portare a inconsistenza e i problemi che vedremo in seguito !
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
- per efficienza, l’indice gestisce le porzioni come se fossero di lunghezza variabile (c’è quindi una entry per ogni porzione libera del disco)
## lista dei blocchi liberi
ad ogni blocco viene assegnato un numero sequenziale, e la lista di questi numeri viene memorizzata in una parte dedicata del disco (per forza riservata, NON come [[#porzioni libere concatenate]])
- poco overhead ! la lista occupa poco spazio
- per avere parti della lista in memoria principale, si può organizzare la lista come stack, e tenere solo la parte alta. in questo modo si può usare pop per allocare, e push per deallocare, e quando lo stack caricato finisce, si prende una nuova parte del disco

# volumi
è un “disco logico”, cioè 
- una partizione di un disco
- più dischi messi insieme e visti come un disco solo (LVM)
è quindi un insieme di settori in memoria secondaria, che possono essere usati dal SO e dalle applicazioni
- i settori non devono essere per forza contigui, ma appariranno come tali al SO e alle applicazioni
un volume potrebbe essere il risultato dell’unione di volumi più piccoli !
# dati e metadati
**dati**: contenuto del file
**metadati**: lista di blocchi liberi, lista di blocchi all’interno dei file, data di ultima modifica, …
- i metadati devono essere su dicsco, perchè devono essere persistenti
- per efficienza, vengono anche tenuti in memoria principale, ma mantere sempre consistenti i metadati in memoria principale e su disco è inefficiente, quindi si fa solo di tanto in tanto, quando il disco è poco usato, e con più aggioronamenti in sieme (buffering)
## jounaling
zona dedicata del disco(**log**) in cui scrivere le operazioni, prima di farne il commit nel file system
- in caso di reboot dopo un crash, basta leggere il log !!
il rispristino del disco, dopo un evento imprevisto (es : computer viene spento all’improvviso per mancanza di corrente, il disco viene rimosso senza dare un appropriato comando (unmount)) viene gestito con il log:
- all’inizio del disco viene scritto un bit che dice se il sistema è stato spento correttamente. a reboot, se il bit è 0, occorre eseguire un programma di ripristino del disco, che confronta il journal allo stato corrente del file system, e corregge le inconsistenze nel file system basandosi sulle operazioni salvate nel journal
ci sono 2 tipi di journal: 
**logico**: copia nel journal **tutti i blocchi** che dovranno poi essere scritti nel file system, inclusi i metadati
- se c’è un crash durante la scrittura nel file system, basta copiare il contenuto del journal al file system al reboot successivo
**fisico**: copia nei journal **solo i metadati** delle operazioni effettuate (es: modifica la lista dei blocchi liberi dopo la cancellazione di un file)
- se c’è un crash, si copiano i metadati dal journal al file system, ma questo può causare corruzione dati, in quanto il contenuto delle operazioni è perso perchè non salvato nel journal
### alternative al journaling
- Soft Updates File Systems: riordina le scritture su file system in modo da non avere mai inconsistenze (o meglio, consente solo alcune tipi di consistenze che non portano a perdite di dati (storage leaks))
- Log-Structured File Systems: l’intero file system è strutturato come un buffer circolare, detto **log**: dati e metadati sono scritti in modo sequenziale, sempre alla fine del log
- Copy-on-Write File Systems: evitano di sovrascrivere dei contenuti nei file: scrivono nuovi contenuti in blocchi vuoti, e poi aggiornano i metadati per puntare ad i nuovi contenuti
# gestione dei file in UNIX
in UNIX ci sono 6 tipi di file:
- normale
- directory
- speciale (i file che mappano su nomi di file i dispositivi di I/O,etc )
- named pipe (per far comunicare i processi tra loro)
- hard link (collegamenti, per dare un nome di file alternativo)
- symbolic links
## inode
sta per “**index node**”, è una struttura dati che contiene informazioni essenziali per un dato file (simile al PCB ! ma per i file)
- ogni inode è associato ad un solo file, ma grazie agli hard link in UNIX un dato inode può essere associato a più **nomi** di file
viene mantenuta dal SO, in memoria principale, una tabella di tutti gli inode corrispondenti a file aperti
- tutti gli altri inode sono in una zona del disco dedicata (**i-list**) (in LINUX si trova all’inizio del disco !)
>[!info] gestione dei file aperti
![[Pasted image 20241123125404.png]]
il kernel unix usa due strutture di controllo separate per gestire file aperti e descrittori inode: 
>- puntatore a descrittori dei file attualmente in uso, salvato nel [[gestione dei processi#process control block|PCB]]
>- una tabella globale di descrittori di file aperti, mantenuto in una apposita struttura dati (quella in mezzo credo)

ogni inode è associato ad un **inode number** (incrementale)
>[!info] rappresentazione inode
![[Pasted image 20241123085909.png]]
>l’inode è la parte a sinistra dell’immagine (contiene quindi diverse informazioni)
>- le celle `direct(0), direct(1)` contengono puntatori a effettivi indirizzi del disco (quindi per i file piccoli (di massimo 13 blocchi), verrano utilizzati SOLO le celle `direct`). esse contengono i blocchi in ordine !
>- esistono poi altri puntatori: `single indirect, double indirect, triple indirect`
>- `single indirect`: punta ad un blocco nel disco, che è indicizzato (contiene le posizioni dei blocchi nel disco che contengono dati del file, come in [[#allocazione indicizzata]])
>- `double indirect`: funziona allo stesso modo del `single indirect` ma con un livello di blocco indicizzato in più
>- `triple indirect`: funziona allo stesso modo del `double indirect` ma con un livello di blocco indicizzato in più
>
> ovviamente non serve tenere conto del fatto che un blocco sia dato o tabella, in quanto, questa informazion si conosce già, in base a da cosa viene puntato il blocco

l’inode contiene:
- 3(3 in Linux, 4 in UNIX) time stamp: ultimo accesso in lettura, ultimo accesso in scrittura, metadati(?)
- identificatore dell’utente proprietario e del gruppo a cui tale utente appartiene
- informazioni sul controllo di accesso (permessi)
- tempo di creazione e di ultimo accesso
- flag utente e flag per il kernel
- numero sequenziale di generazione del file
- dimensione delle informazioni aggiuntive
- altri attributi (controllo di accesso e altro)
- dimensione
- numero di blocchi, o numero di file
- dimensione dei blocchi
- sequenze di puntatori a blocchi
l’allocazione è quindi: a blocchi, dinamica, e indicizzata (anche se parte dell’indice è memorizzata nell’inode)
>[!info] risultati con inode
![[Pasted image 20241123091622.png]]
### directory
le directory sono una lista di coppie (nome di file, puntatore ad inode)
>[!example]
quind se un file si chiama “pippo” e il suo inode number è 100, basta andare nella **i-list** e prendere l’inode 100 (che è facile, basta saltare i primi 99 (che immagino abbiano tutti la stessa dimensione, quindi dovrebbe essere veramente facile)), e una volta ottenuto l’inode abbiamo tutte le informazioni che ci servono sul file

più file sono messi in una directory, più grande è la directory (anche se di solito bastano i puntatori `direct` per gestire una directory)
>[!info]  rappresentazione directory
![[Pasted image 20241123084739.png|350]]

>[!info] regioni del volume
![[Pasted image 20241123124845.png]]
**regione boot sector**: simile a blocco di boot per [[#FAT]], contienele informazioni e i dati necessari per il bootstrap
**regione superblock**: contiene informazioni sui metadati del filesystem (dimensione della partizione, dimensione dei blocchi, puntatore alla lista dei blocchi liberi, etc). esistono copie multiple di questa regione, in caso di corruzione, e sono salvate in gruppi di blocchi sparsi nel file system (la prima copia è sempre in una parte prefissata della partizione, in modo da consentire recovery in modo semplice)
**regione lista degli inode**: la i-list !!
## gestione file condivisi
come gestire i file che devono essere condivis in più directory ? fare una copia del file è inutilmente costoso. esisono 2 soluzioni:
**symbolic links**: esiste un solo descrittore (inode) del file originale, e i symlinks contengono il cammino completo sul filesystem verso tale file (stanno quindi dove dove NON si trova il file, altrimenti sarebbe inutile)
- possono esiste symlinks a file non più esistenti
**hard links**: sono un puntatore diretto al descrittore del file originale(inode), e il file condiviso non può essere cancellato finchè esiste un hard link collegato ad esso
- per gestire ciò, l’inode contiene un contatore dei file che lo referenziano
# gestione dei file su windows
esistono 2 tipi di file system su windows:
- FAT(file sytem vecchio): si basa su allocazione concatenata, con blocchi (chiamati **cluster**) di dimensione fissa
- NTFS(file system nuovo): si basa su alloaczione **con bitmap**, con cluster di dimensione fissa
## FAT
è molto limitato (andava bene per i vecchi dischi, soprattutto i floppy), ma è usato ancora oggi per le chiavette USB
- inoltre l’approccio in sè non è scalabile, in quanto la FAT stessa può occupare molto spazio
esiste una parte di disco usata per l’area riservata, che contiene dei dati
la FAT (**file allocation table**) è memorizzata all’inizio della partizione su disco, e ce ne sono 2 copie (per ridondanza)
- essa è costituita da una sola colonna, ed ogni riga contiene un valore intero a 12,16 o 32 bit (FAT-12, FAT-16, FAT-32)
- ci sono tante righe quanti ci sono cluster, e ogni cluster ha dimensione fissa, che viene scelta dall’utente in fase di formattazione e può andare da 2KB a 32KB
	- di conseguenza, la tabella cresce con la grandezza della partizione
- ogni cluster è costituito da settori di disco contigui
>[!info] rappresentazione FAT
![[Pasted image 20241123101452.png]]
ogni file ha una specie di inode che contiene il nome del file e l’entry (nella FAT) collegata al cluster che contiene quel file
il FAT funziona similarmente alla [[#allocazione concatenata]], ma i puntatori non sono dentro la partizione, ma sono i valori della FAT
per ogni entry della FAT, ci possono essere 3 opzioni riguardo al valore di una riga (cluster): 
>- se la i-esima entry è zero, il blocco i-esimo è libero
>- se la i-esima entry non è zero e non è un valore speciale, allora il cluster associato contiene il file e il valore della riga è la prossima riga che contiene il file
>- ci sono diversi valori speciali (per valori riservati, cluster danneggiati, etc) ma a noi interessa il valore speciale `FFFF` (nel FAT-16, cioè tutti 1), che vuol dire che quella riga era l’ultimo blocco del file

ciò consente di identificare i blocchi di un file e accedervi seguendo sequenzialmente i collegamenti nella FAT
>[!warning] la parte della FAT relativa ai file aperti deve essere sempre mantenuta interamente in memoria principale
### regioni del volume
>[!info] rappresentazione delle regioni di un volume FAT
![[Pasted image 20241123101018.png]]
>- **regione boot sector**: contiene informazioni necessarie per l’accesso al volume, tra cui il tipo (del volume ? \\QUESTION) e puntatori alle altre sezioni del volume
> - **regione FAT**: due copie della FAT, in caso la tabella principale sia corrotta
> - **regione root directory**: è una **directory table** che contiene tutti i file entry per la directory root del sistema (la dimensione della table è limitata a 256 entry in FAT-12 e FAT-16, mentre in FAT-32 è inclusa nella regione dati, insieme a file e directory normali, e non ha limitazioni sulla dimensione)
> - **regione dati**: è la regione del volume in cui sono effettivamente contenuti i dati dei file e directory

per le directory, al posto di essere una lista con entrate del tipo “nomefile-numero inode", le directory sono liste con entrate del tipo “nomefile-insieme di dati”, e l’insieme di dati è il seguente:
>[!info] 
>![[Pasted image 20241123115042.png|900x500]]
>- 32 byte in tutto
>- quindi l’illustrazione nella rappresentazione del FAT è una semplificazione !
### limitazioni
- supporta file di dimensione massima 4GB (32 bit nel campo dimensione file nelle directory)
- non implementa journaling
- non consente alcun meccanismo di controllo di accessi ai file/directory
- limite dimensione partizioni: 2TB ($2^32$ settori da 512B)
## NTFS
 sta per **new technology file system**, è il file sytem adottato a partire da Windows NT in poi
 - usa UNICODE per l’encoding dei nomi dei file, con 255 caratteri come limite massimo
 - i file sono definiti da un insieme di attributi, che sono rappresentati come un **byte stream**
 - supporta hard e soft link 
 - implementa journaling
 >[!info] formato del volume
 ![[Pasted image 20241123121915.png]]
 **regione boot sector**: basata sull’equivalente FAT, alcuni campi sono in posizioni diverse ma per il resto è uguale
 **master file table**: contiene la MTF
### MFT
la **master file table** è la principale struttura dati del file system: è unica per ciascun volume, ed è implementata come un file
- la MFT è una sequenza lineare di record (massimo $2^{48}$, la cui dimensione va da 1KB 4KB(penso funzioni a scelta in fase di formattazione come in FAT)), e ogni record descrive un file
#### record di MFT
ogni record è identificato da un puntatore di 48bit, mentre i rimanenti 16bit sono usati come numero di sequenza
- ogni record contiene una lista di attributi (nella forma “attributo(intero, che indica il tipo di attributo)-valore(sequenza di byte)”)
- il contenuto di un file è anch’esso un attributo !(`$DATA`)
- il valore dell’attributo può essere incluso direttamente nel record (**attributo residente**), oppure può essere un puntatore ad un record remoto (**attributo non residente**) se il valore dell’attributo è troppo grande e non sta nel record (il caso di `$DATA`)
i primi 27 record sono riservati per i metadati del file system:
- il record 0 descrive l’MFT stesso (in particolare, tutti i file nel volume (damn))
- il record 1 contiene una copia dei primi record dell’MFT, in modo non residente
- il record 2 contiene le informazioni di journaling (metadata-only !)
- il record 3 ha le informazioni sul volume (id, label, versione del file system, etc)
- il record 4 è una tabella degli attributi usati nella MFT (definisce un intero per ogni attributo)
- il record 6 definisce la lista dei blocchi liberi, usando una bitmap
dal record 28 in poi, ci sono i descrittori dei file normali (sempre in forma di record)
>[!info] rappresentazione MFT
>![[Pasted image 20241123123045.png|500]]
#### files in MFT
NTFS cerca sempre di assegnare ad un file sequenze contigue di blocchi
- per i file piccoli (< 1KB), i dati sono salvati direttamente nell’MFT (nel record penso intenda, in particolare nell’attributo `$DATA`)
- per i file grandi, il valore dell’attributo(`$DATA`) indica la sequenza ordinata dei blocchi sul disco dove risiede il file
per ogni file, esiste un record base nell’MFT
- \\QUESTION
>[!info] record base
![[Pasted image 20241123123952.png]]
in questo esempio, il file ha dimensione 9 blocchi, divisi in 3 **run** (dal blocco 20, 4 blocchi contigui hanno dati del file. dal blocco 64, 2 blocchi contigui hanno dati del file, etc)
il record base è un descrittore singolo sufficiente per l’intera lista di run, infatti consente di descrivere file di dimensione potenzialmente illimitata: dipende tutto dalla contiguità dei blocchi
>- con blocchi da 1KB, un file da 20GB diviso in 20 run da 1M di blocchi ognuna, richiede 20 coppie di 64 bit (ogni coppia determina una run), più un’altra coppia \\QUESTION(credo c’entri con lo 0), quindi in totale il record base occupa **336B**
>- sempre con blocchi da 1KB, un file da 64KB, con 64 run da 1 blocco occupa **1040B**

file di larghe dimensioni (e/o sfortunati con la contiguità), possono necessitare di più record, e per gestire ciò NFTS usa una tecnica simile agli inode di UNIX:
- il record base ““diventa” un puntatore a $N$ record secondari, ed eventuale spazio rimanente nel record base contiene le prime sequenze del file (very cool !!)
- se i record estesi non rientrano in MFT per mancanza di spazio, vengono trattati come attributo non residente, e quindi salvati in un file dedicato, con un apposito record salvato nell’MFT
>[!info] record con estensioni
![[Pasted image 20241123124504.png]]

# gestione file system in Linux
il file system originario di Linux è **ext2**, che è l’evoluzione (se non la copia) del file system originario di UNIlX
in seguito, è stato creato **ext3**, che aggiungeva il journaling all’ext2
ancora in seguito, è stato creato **ext4**(l’attualmente più utilizzato), una versione migliore dell’ext3 in grado di memorizzare singoli file più grandi di 2TB e filesystem più grandi di 16TB
in generale, quindi, il file sytem di Linux è fortemente basato sugli inode, che sono memorizzati nella parte iniziale del file sytem
>[!info] linux può tranquillamente utilizzare i filesystem FAT e NTFS
>sorge un problema, in quanto FAT e NTFS non sono basati su inode: per rimediare, gli inode vengono creati on-the-fly su una cache quando vengono aperti i file o viene letta una directory