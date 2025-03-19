---
created: 2025-03-19T18:58
updated: 2025-03-19T19:10
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
- **user agent**: usato per scrivere e inviare un messaggio o leggerlo
- **message transfer agent**: usato per trasferire il messaggio attraverso Internet
- **message access agent**: usato per leggere la mail in arrivo
come funziona il servizio mail ?

2 protocolli x realizzare posta elettronica !

MTA: server mittente parla direttamnete al sever destinatario ! (a livello di comunicazione/connessione)

se il client dovesse mandare + mail, nella slide 21 si clicla nella parte da DAtA in poi