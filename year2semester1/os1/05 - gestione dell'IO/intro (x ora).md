\\TODO index
la gestione dell’IO è problematica per la progettazzione di SO, in quanto:
- c’è una grande varietà di dispositivi di IO
- c’è una grande varietà di applicazioni che li usano
- hanno poche caratteristiche comuni, e dobbiamo tenere conto di tali diversità, e gestire l’IO nel modo più unificato possibile
## categorie di dispositivi di IO
**leggibili dall’utente**:
- dispositivi usati per la comunicazione diretta con l’utente: stampanti, terminali (monitor, tastiera, mouse), joystick, etc.
**leggibili dalla macchina**
- dispositivi usati per la comunicazione con materiale elettronico: dischi, chiave USB, sensori, controllori, etc
**per la comunicazione**:
- dispositivi usati per la comunicazione con dispositivi remoti: modem, schede Ethernet, Wi-FI
# funzionamento dei dispositivi I/O
## input
un dispositivo di input prevede di essere interrogato sul valore di una certa grandezza fisica al suo interno
- tastiera: codice unicode degli ultimi tasti premuti
- mouse: cordinate dell’ultimo spostamento effettuato, quali tasti sono stati premuti
- disco: valore dei bit che si trovano in una certa posizione al suo interno
quando un processo effettua una syscall `read` su un dispositivo, vuole conoscere tale dato per poterlo elaborare
## output
un dispositivo di output prevede di poter cambiare il valore di una certa grandezza fisica al suo interno
- monitor moderno: valore RGB di tutti i suoi pixel
- stampante moderna: PDF di un file da stampare
- disco: valore dei bit che devono sovrascrivere quelli che si trovano in una certa posizione al suo interno
un processo che effettua una syscall `write` su un dispositivo di output vuole cambiare qualcosa

ci sono quindi, minimalmente, due syscall `read` e `write`, e tra i loro argomenti c’è un indentificato del dispositivo a cui ci si riferisce
- quando viene effettuata una delle due syscall, entra in gioco il kernel(avviene quindi un mode switch), che comanda l’inizializzazione del trasferimento di informazioni, mette il processo in `BLOCKED` e passa ad altro

>[!info] la parte del kernel che gestisce un particolare dispositivo di IO si chiama driver

spesso il trasferimento si fa con [[ciclo di esecuzione e interruzioni#accessso diretto in memoria|DMA]]
a trasferimento completato, arriva l’interrupt, si termina l’operaione e il processo torna ready
- ci possono essere problemi! (bad block su disco, etc)
# differenze tra dispositivi IO
i dispositivi IO possono differire sotto molti aspetti:
### data rate
>[!figure] ![[Pasted image 20241115072954.png]]
>come si può vedere, ci possono essere differenze anche elevate sulla velocità dei dispositivi
### applicazioni
- i dischi sono usati per memorizzare files, e richiedono un software(applicazione) per la gestione dei files
- ma i dischi sono usati anche per la memoria virtuale, per la quale serve altro software apposito
- un terminale (tastiera, mouse, etc) usato da un amministratore di sitema dovrebbe avere una priorità più alta
### complessità del controllo
- tastiera o mouse richiedono un controllo molto semplice, mentre una stampante è più complicata, e il disco è tra le cose più complicate da controllare
fortunatamente, non si fa tutto con il software, e molte cose vengono controllate da un hardware dedicato
- i dispositivi I/O(moderni ?) hanno un controller, un chip che permette di effettuare le operazioni necessarie
### unità di trasferimento dati
i dati possono essere trasferiti:
- in blocchi di byte di lunghezza fissa
- come un flusso di byte
### rappresentazione dei dati
i dati sono rappresntati secondo codifiche diverse in dispositivi diversi
- una vecchia tastiera magari li rappresenta in ASCII puro, ma una moderna li rappresenta in UNICODE
little-endian, big-endian
### condizioni d’errore
la natura degli errori varia di molto da dispositivo a dispositivo
- nel modo in cui gli errori vengono notificati
- sulle loro conseguenze (fataili?ignorabili)
- su come possono essere gestiti

# DMA 
il DMA (**direct memory access**) permete di effettuare IO:
- direttamente in memoria (senza passare per la CPU)
- con interruzioni
ed è la soluzione usata da tutti i sistemi moderni
il suo funzionamento è il seguente: 
\\TODO
DMa non ci fa perdere tempo perchè una volta dato il comando fa tutto in automatico e ci fa sapere quando ha finito (DMA dispositivo hardware, un chip solo per le operazioni specifiche per trasferimento dati I/O)
# organizzazione della funzione di I/O
ultime evoluzioni della funzione di I/O sono:
5. DMA: blocchi di dati viaggiano tra dispositivo e memoria, senza usare il procesore ( il processore fa qualosa solo all’inizio e alla fine di un’operazione di IO)
6. I/O channel: il modulo di IO diventa un processore separato, a cui il processore (CPU) manda comandi per eseguire certi programmi di I/O in memoria principale
7. I/O processor: il processore di 6. ha una sua memoria dedicata (come cache, quindi fa operazioni senza dover usare la RAM). x esempio la VRAM !!
>[!info] il chipset
> il chipset è una collezione di componenti elettroniche (un chip), che va a implementare le connessioni con i diversi dispositivi I/O, porte SATA, etc…
> 
>![[Pasted image 20241116131705.png]]
