---
created: 2025-03-19T18:58
updated: 2025-03-19T19:29
---
# FTP
**FTP** (file transfer protocol) è un programma di trasferimento file da/a un host remoto. segue il modello client/server:
- **client**: il lato che inizia il trasferimento 
- **server**: host remoto
>[!example] comunicazione FTP
![[Pasted image 20250319110917.png]]
>- quando un utente fornisce il nome dell’host remoto, con il comando `ftp nomeHost`, il processo client FTP stabilisce una connessione TCP sulla porta 21 con il processo server FTP
>- stabilita la connessione, il client fornisce nome utente e password che vengono inviate sulla connessione TCP come parte dei comandi
>- ottenuta l’autorizzazione del server, il client può inviare uno o più file memorizzati nel file system locale verso quello remoto, o viceversa

## connessioni FTP
durante la comunicazione con FTP, avvengono 2 connessioni:
### connessione di controllo
si occupa delle informazioni di controllo del trasferimento: identificativo utente, password, comandi per cambiare dir, comandi per richiedere invio/ricezione di file, …
usa regole molto semplici, così che lo scambio di informazioni si riduce allo scambio di una riga di comando per ogni interazione !
- avviene sulla **porta 21**, e viene aperta con il comando `ftp nomeHost`
- tutti i comandi eseguiti dall’utente vengono trasferiti sulla connessione di controllo
- è detta una connessione **out of band**(fuori banda): utilizza un canale separato rispetto alla connesione dati per gestire i comandi e le risposte tra client e server (HTTP per esempio usa un solo canale x tutto !)
- il server FTP mantiene lo “stato”: dir corrente, autenticazione precedente
- è **persistente**
### connessione dati
si occupa del trasferimento del file: quando il server riceve un comando per trasferire un file (che sia `get` o `put`), apre una connessione dati TCP sulla **porta 20** con il client, e dopo il trasferimento di un file, il server chiude la connessione
- la connessione viene quindi usata solo per il vero e proprio invio del file, e si crea una nuova connessione per ogni file trasferito all’interno della sessione
- è quindi **non persistente**
>[!figure] connessioni FTP
![[Pasted image 20250319113753.png]]

## comandi e risposte FTP
### principali comandi FTP

| comando | argomenti              | descrizione                                     |
| ------- | ---------------------- | ----------------------------------------------- |
| `ABOR`  |                        | interruzione del comando precedente             |
| `CDUP`  |                        | sale di un livello nell’albero delle dir        |
| `CWD`   | nome della dir         | cambia la dir corrente                          |
| `DELE`  | nome del file          | cancella il file                                |
| `LIST`  | nome della dir         | elenca il contenuto della dir                   |
| `MKD`   | nome della dir         | crea una nuova dir                              |
| `PASS`  | password utente        | password                                        |
| `PASV`  |                        | il server sceglie la porta                      |
| `PORT`  | numero di porta        | il client sceglie la porta                      |
| `PWD`   |                        | mostra il nome della directory corrente         |
| `QUIT`  |                        | uscita dal sistema                              |
| `RETR`  | nome di uno o più file | trasferisce uno o più file dal server al client |
| `RMD`   | nome della dir         | cancella la dir                                 |
| `RNTO`  | nome (del nuovo) file  | cambia il nome del file                         |
| `STOR`  | nome di uno o più file | trasferisce uno o più file dal client al server |
| `USER`  | identificativo         | identificazione dell’utente                     |
### esempi di risposte FTP
le risposte sono composta da due parti:
- un numero di 3 cifre: il codice della risposta
- un testo: i parametri necessari o informazioni supplementari
la tabella seguente riporta solo i codici

| codice | descrizione                                        |
| ------ | -------------------------------------------------- |
| 125    | connessione dati aperta                            |
| 150    | stato del file OK                                  |
| 200    | comando OK                                         |
| 220    | servizio pronto                                    |
| 221    | servizio in chiusura                               |
| 225    | connessione dati aperta (?)                        |
| 226    | connesione dati in chiusura                        |
| 230    | login dell’utente OK                               |
| 250    | azione sul file OK                                 |
| 331    | nome dell’utente OK: in attesa della password      |
| 425    | non è possibile aprire la connesione dati          |
| 450    | azione sul file non eseguita; file non disponibile |
| 452    | azione interrotta; spazio insufficiente            |
| 500    | errore di sintassi; comando non riconosciuto       |
| 501    | errore di sintassi nei parametri o negli argomenti |
| 530    | login dell’utente fallito                          |

>[!example] esempio richiesta e risposta FTP
![[Pasted image 20250319190752.png]]
# posta elettronica
esistono 3 componenti principali nel funzionamento della posta elettronica:
- **user agent**(UA): detto anche “mail reader”, è usato per scrivere e inviare un messaggio o leggerlo. viene attivato dall’utente o da un timer: se c’è una nuova email informa l’utente, ma si occupa anche di composizione, editing e lettura dei messaggi di posta elettronica. inoltre passa al MTA il messaggio da inviare
	- esempi: eudora, outlook, thunderbird
- **message transfer agent**(MTA): usato per trasferire il messaggio attraverso Internet
>[!info] come comunicano gli MTA ?
![[Pasted image 20250319191644.png]]

>gli MTA comunicano attraverso il protocollo **SMTP**(simple mail transfer protocol), e sono costituiti da:
>- casella di posta: contiene i messaggi in arrivo per l’utente
>- coda di messaggi: i messaggi da trasmettere (tentativi ogni $x$ minuti per alcuni giorni)
- **message access agent**(MAA): usato per leggere la mail in arrivo
vediamo il loro funzionamento nell’esempio:
>[!example] l’esempio
![[Pasted image 20250319191229.png]]

## SMTP
il protocollo **SMTP** usa [[06 - livello applicazione; FTP, SMTP#FTP|FTP]] per trasferire in modo affidabile i messaggi di posta elettronica dal client al server, utilizzando la porta 25
- il trasferimento è diretto: dal server trasmittente al server ricevente, e si divide in 3 fasi:
	- handshaking
	- trasferimento di messaggi
	- chiusura
i messaggi devono essere in formato ASCII, così come i comandi inviati durante la comunicazione. le risposte invece sono composte da codice di stato ed espressione
>[!example] esempio …
>1. alice usa il suo agente utente per comporre il messaggio da inviare a `rob@someschool.edu`
>2. l’agente utente di alice invia un messaggio al server di posta di alice: il messagio è posto nella coda di messaggi
>3. il lato client di SMTP apre una connessione TCP con il server di posta di rob
>4. il client SMTP invia il messaggio di alice sulla connessione TCP
>5. il server di posta di rob riceve il messaggio e lo pone nella casella di posta di rob
>6. rob invoca il suo agente utente per leggere il messagio
![[Pasted image 20250319192341.png]]
![[Pasted image 20250319192356.png]]


come funziona il servizio mail ?

2 protocolli x realizzare posta elettronica !

MTA: server mittente parla direttamnete al sever destinatario ! (a livello di comunicazione/connessione)

se il client dovesse mandare + mail, nella slide 21 si clicla nella parte da DAtA in poi