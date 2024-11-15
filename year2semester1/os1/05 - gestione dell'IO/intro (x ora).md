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
- quando viene effettuata una delle due syscall, entra in gioco il kernel, che comanda l’inizializzazione del trasferimento di informazioni, mette il processo in `BLOCKED` e passa ad altro

>[!info] la parte del kernel che gestisce un particolare dispositivo di IO si chiama driver

il SO ha diversi livelli di interazione con i dispositivi I/O, con funzioni di diverso livello
driver: moduli di sistema che implementano 
- spesso il trasferimento si fa con DMA
a trasferimento completato, arriva l’interrupt, si termina l’operaione e il processo torna ready
- ci possono essere problemi

processo chiama funzione di sistema (quindi mode switch)

il DMA si occupad i andare a prendere i dati, li mette in una zona di memoria delegata, etc uhhh cmq con il DMA si complica 