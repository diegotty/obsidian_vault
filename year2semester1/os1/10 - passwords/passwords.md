---
created: 2024-12-17
related to: 
updated: 2024-12-18T07:38
---
una password è un dato segreto, tipicamente utilizzato per identificare un utente, e garantire l’accesso sicuro
# password in Linux
per gestire utenti e le relative password, Linux usa 2 file:
- `\etc\passwd`
- `\etc\shadow`
entrambi sono normali file di testo con una sintassi simile, ma hanno funzionalità e permessi diversi
- per ogni riga (utente) in `\etc\passwd`, esiste un a riga in `\etc\shadow` che indica la sua password
- originariamente, esisteva solo il file *passwd*, che includeva la password dell’utente in plaintext….. la struttura è stata cambiata x ovvi motivi
## \etc\passwd
è un file in plaintex, che contiene l’intera lista di utenti presenti nel sistema
- include non solo gli utenti “normali”, ma anche utenti standard di sistemi e utenti speciali (ad esempio l’utente *nobody*, che solitamente è usato per dare il minimo set di permessi possibili ad un processo)
di default, `\etc\passwd` ha i seguenti permessi:
```
-rw-r--r-- 1 root root 2659 Dec 22 12:21 /etc/passwd
```

ciascuna riga del file `passwd` indica informazioni fondamentali su ciascun utente del sistema, ed ha il seguente formato:
>[!info] formato riga di passwd
![[Pasted image 20241218070946.png]]
>1. username: stringa alfanumerica di max 32 caratteri
>2. password: il carattere “x” indica che l’hash della password è nel file `\etc\shadow`
>3. uid(User ID): ogni utente nel sistema ha un user id numero assegnato. (in particolare l’uid 0 indica sempre l’utente root, e i numeri 1-999 sono utilizzati dagli account predefiniti e di sistema) 
>4. gid(Group ID): il gruppo a cui appartiene l’utente (ogni volta che viene creato un utente, esso è assegnato ad un *primary group*, che è poi il gruppo che il SO assegna ai file creati dall’utente). la descrizione dei gruppi si trova in `\etc\group`
>5. GECOS: campo descrittivo che contiene informazioni generali sull’utent
>6. home directory: path assoluto alla home directory dell’utente
>7. shell: path assoluto alla command shell usata dall’utente

## \etc\shadow
il file `shadow` è un plaintext file, contenente, per ciascun utente del sistema, l’hash della sua password ed altre informazioni aggiuntive
- data la criticità delle informazioni contenute, sottrarre e decifrare lo shadow file è spesso uno degli obiettivi principali di un attaccante, e conseguentemente ha permessi molto più restrittivi di passwd
di default, ha i seguenti permessi:
```
-rw-r----- 1 root root 2659 Dec 22 12:21 /etc/shadow
```

>[!info] formato riga di \etc\shadow
![[Pasted image 20241218072100.png]]
>1. username: nome dell’utente a cui la password appartiene (definito in passwd)
>2. password: password dell’utente salvato usando il Modular Crypt Format
>3. last changed: data dell’ultimo cambiamento della password, espresso in giorni trascorsi dall’Unix Epoch(01/01/1970). se il valore di questo campo è 0, vuol dire che l’utente deve cambiare password al suo prossimo accesso, mentre se il campo è vuoto, vuol dire che le funzioni di invecchiamento della password sono disabilitate
>4. min age: il minimo numero di giorni dall’ultimo cambio prima che la password possa essere nuovamente cambiata
>5. max age: il massimo numero di giorni dopo dei quali è necessario cambiare la password
>6. warn: quanti giorni prima della scadenza della password va avvisato l’utente
esistono anche altri 2 campi (possiamo vedere dall’imagine che in questo caaso sono vuoti): inactive (numero di giorni dopo la scadenza della password dopo di chè l’account viene disabilitato), expire(data di scadenza dell’account)
## modular crypt format
è il formatoo usato nello shadow file per salvare gli hash delle password, ed è il seguente formato:
$$$ID$salt$hash$$
**ID**: indica l’algoritmo di salting usato per questa password (ID=1 corrisponde a MD5, ID = 6 corrisponde all’algritmo SHA512, e ne esistono tanti altri: blowfish, SHA256, …)
**salt**: senquenza randomica data in input ad una funzione hash, per garantire un risultato unico, anche se la password data in input è la stessa
**hash**: hash della password, calcolato con l’algoritmo ID e il salt
## hash functions
>[!info] rappresentazione hash function
>![[Pasted image 20241218073629.png]]

la funzione hash trasforma un input di lunghezza variabile, in un output di lunghezza fissa, in maniera **deterministica**:
- cioè, data la stessa stringa in input, darà sempre lo stesso digest
# attacchi a password
# sviluppi futuri