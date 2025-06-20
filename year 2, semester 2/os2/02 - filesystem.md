---
related to: 
created: 2025-03-02T17:41
updated: 2025-06-19T19:04
completed: true
---
>[!index]
>- [filesytem](#filesytem)
>	- [mounting](#mounting)
>		- [tipi di filesystem](#tipi%20di%20filesystem)
>	- [file `passwd` e `group`](#file%20%60passwd%60%20e%20%60group%60)
>	- [i file](#i%20file)
>	- [`ls`](#%60ls%60)
>	- [permessi di accesso a file](#permessi%20di%20accesso%20a%20file)
>		- [permessi speciali](#permessi%20speciali)
>			- [sticky bit](#sticky%20bit)
>			- [setuid bit](#setuid%20bit)
>			- [setgid bit](#setgid%20bit)
>	- [comandi](#comandi)
# filesytem
i file regolari contengono sequenze di bit dell’area di memoria sulla quale c’è il filesystem 
- possono essere ASCII o binari
esiston anche **file non regolari, o speciali**, che modellano unità di I/O
- possono essere a blocchi/caratteri
>[!tip] ~ equivale a /home/userX
>ed è la home directory del current user

>[!info] fun facts .. 
>- quando si usa `cd`, `/./` fa rimanere allo stesso posto. (huh.)
>- path assoluto è valido qualunque sia la `cwd`, mentre il path relativo può non essere valido quando si cambia `cwd`
## mounting
li filesystem root (`/`) contiene elementi eterogenei: 
- disco interno
- filesystem su disco esterno
- filesystem di rete
- filesytem virtuali (usati dal kernel per gestire risorse)
- filesystem in memoria principale..
una qualsiasi directory $D$ dell’albero gerarchico può diventare il punto di mount per un altro (nuovo) filesystem $F$ se e solo se la directory root di $F$ può essere accessibilie di $D$
- se $D$ è vuota, dopo il mount conterrà $F$
- se $D$ non è vuota, dopo il mout conterrà $F$, ma ciò non significa che i dati che vi erano dentro sono persi: saranno di nuovo accessibili dopo l’unmount di $F$
>[!info] partizioni e mounting
un singolo disco può essere diviso in due o più partizioni ! e ciò ha molto senso. presumiamo che una partizione $A$ contenta il SO e la partizione $B$ contenga i dati degli utenti.
>- se devo reinstallare il SO in $A$, non tocco i dati e mi basta rimontare $B$, e se voglio installare diverse distro di linux in altre partizioni (es: $C$), posso allo stesso modo avere i dati degli utenti senza copiarli, ma semplicemente montando $B$
### tipi di filesystem
come sappiamo da [[filesystem in UNIX, Windows, Linux|os1]], esistono diversi tipi di filesystem. dal punto di vista del programmatore, il tipo di filesystem definisce la codifica dei dati, mentre dal punto di vista dell’utente cambiano la dimensione massima di una partizione, di un file, di un nome di file, e se usa il journaling oppure no
>[!example] filesystem non linux
>esistono ovviamente filesystem “non linux”: NTFS, MSDOS, FAT32, FAT64
>- FAT ed NTFS possono essere montati su di un filesystem linux (lo sappiamo già !)

`mount` è il comando per montare un filesystem, e visualizzare i filesystem montati
## file `passwd` e `group`
si trovano entrambi in `/etc/`, e il loro uso rappresenta una delle filosofie di linux: usare file di testo con una struttura definita e conosciuta dai programmi che devono interagire con quei file (in questo caso, `adduser` conosce la struttura di questi due file)
la struttura è stata spiegata in [[sicurezza]]
## i file
come visto in [[filesystem in UNIX, Windows, Linux#inode]], ogni file nel filesystem è rappresentato da una struttura dati **inode**, e ogni file è unicamente identificato da un **inode number**. 
- la cancellazione di un file libera l’inode number che verrà riutilizzato quando necessario per un nuovo file
>[!info] come viene seguito un path
![[Pasted image 20250315190353.png]]
## `ls`
per visualizzare gli inode, basta usare il flag `-i` con il comando `ls`
>[!info] ls -l
![[Pasted image 20250315190629.png]]
>- la dimensione (quinta colonna) indica quanto effettivamente è occupato dal file: per le directory, la dimensione del file speciale, contenente la lista di coppie è solitamente 4kb, ma più file ci sono più la dimensione è grande (es: `/lost+found`)
>- il numero dopo i diritti di accesso invece indica il numero di directory all’interno della directory, in cui vengono contate anche `.` e`..` (infatti per il file è 1)
>- la prima riga invece, `totale 44` indica la dimensione della directory in blocchi su disco (tipicamente un blocco ha dimensione tra 1kb e 4kb). ma solo della directory attuale, non per tutto il sottoalbero !! per ogni sottodirectory, considera solo la dimensione del file speciale contenente le coppie
## permessi di accesso a file
it pains me to write it a 3rd time, here is a table
>[!info]- @@@@
![[Pasted image 20250315191911.png]]
![[Pasted image 20250315191857.png]]
### permessi speciali
esistono permessi speciali, applicabili a file e directory
#### sticky bit
viene applicato su directory (inutile su file) per correggere il comportamento di `w+x` che permette la cancellazione di file se si hanno i permessi di scrittura su essi
- grazie allo sticky bit, per cancellare un file $f$, un utente non proprietario **deve** avere i permessi di scrittura su $f$, e non solo sulla directory a cui $f$ appartiene
lo sticky bit viene visualizzato al posto del bit di esecuzione nella terna `other`
#### setuid bit
si usa solo per file eseguibili: quando vengono eseguiti, i privilegi con cui opera il corrispondente processo non sono quelli dell’utente che esegue il file, bensì quelli dell’utente proprietario del file ! (se il proprietario è `root`, il processo viene eseguito con i privilegi di `root` indipendentemente da chi lo ha eseguito)
- es: il comando `passwd` ha il setuid, che permette ad un utente di modificare la propria password (il proprietario del comando è root)
il setuid bit viene visualizzato al posto del bit di esecuzione nella terna `user`
#### setgid bit
simile al setuid bit, ma in questo caso i privilegi del processo sono quelli del gruppo che è proprietario del file
- può essere applicato anche ad una directory, e allora ogni file creato al suo interno ha il gruppo della directory, anzichè quello primario di chi crea i files !
il setgid bit viene visualizzato al posto del bit di esecuzione nella terna `group`
## comandi
>[!info] comandi
>### $\verb |chmod [mode]|$
>può settare i diritti di accesso in formato ottale in cui il primo numero rappresenta setuid(4)+setguid(2)+sticky(1)
>### $\verb |chmod [mode simbolica]|$
>può settare i diritti di acesso usando simboli con aggiunta di `[lettere tra {ugo}] [+-=] [0 o più lettere tra {rxwXst}]`
>### $\verb |chown [-R] propietario {file}|$
>cambia proprietario del file, se il file è una directory con `-R` si applica questo a tutte le sottodirectory
>### $\verb |chgrp [-R] gruppo {file}|$
>cambia il gruppo a cui il file appartiene,se il file è una directory con `-R` si applica questo a tutte le sottodirectory
>### $\verb |umask [mode]|$
>setta la maschera dei file(cioè i diritti di accesso al fileo alle directory nel momento della loro creazione) a `mode`
>- per i file però, il diritto di esecuzione non viene settato (quindi le opzioni speciali non hanno effetto, in quanto prendono il posto del bit di esecuzione in terne diverse)
>### $\verb |cp [-r] [-i] [-a] [-u] {filesorgenti} filedestinazione|$
>- `-r`: recursive, per directory
>- `-i` interactive, per essere avvisati in caso di sovrascrittura
>- `-u` la sovrascrittura avviene solo sel l’`mtime` del sorgente è più recente di quello della destinazione (cool !)
>### $\verb |mv [-i] [-u] [-f] {filesorgenti} filedestinazione|$
>sposta un file o lo rinomina !
>- `-i` e`-u` hanno lo stesso significato che hanno in `cp`
>### $\verb |rm [-f] [-i] [-r] {file}|$
>- `-i` e`-u` hanno lo stesso significato che hanno in `cp`
>- `-f` forza la cancellazione (senza chiedere)
>### $\verb | ln [-s] sorgente [destinazione]|$
>-`-s` per symbolic link, altrimenti hard link(copia effettiva) !
>### $\verb |touch [-a] [-m] [-t timestamp] {file}|$
>- serve per creare un file, o modificare il suo timestamp
>- può essere applicato anche su dir
>- `-t` setta il timestamp desiderato
>### $\verb |du [-c] [-s] [-a] [-h] [--exclude=PATTERN] [files]|$
>- calcola la dimensione dei file e/o dir dati come argomento 
>### $\verb |df [-h] [-l] [-i] [file]|$
>- mostra la dimensione e l’atttuale uso del filesystem
>### $\verb | dd [opzioni] |$
>**convert and copy a file**
>- serve per creare file in modo elaborato: le opzioni sono una sequenza `variabile=valore`, e le variabili più importanti sono:
>- - `bs`: dimensione di un singolo blocco in lettura/scrittura
>- `count`: numero di blocchi da copiare
>- `convert`: il valore specifica una conversione, per esempio di codifica (da miniuscolo a maiuscolo e viceversa (??))
>- `if` file di input (se non dato, legge da tastiera)
>- `of` file di output (se non dato, scrive su schermo)
>oltre che per le conversioni, si usa per copiare file speciali che non possono essere copiati con `cp`
>### $\verb |mkfs [-t type fsoptions] device|$
- crea un filesystem su device (prepara i file a memorizzare file secondo un dato formato, es [[filesystem in UNIX, Windows, Linux#gestione file system in Linux|ext4]])