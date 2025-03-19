---
related to: "[[03 - introduzione allo stack protocollare TCP-IP]]"
created: 2025-03-02T17:41
updated: 2025-03-19T11:38
completed: false
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


| comando | argomenti | descrizione |
| ------- | --------- | ----------- |
| `ABOR`  |           |             |
| `CDUP`  |           |             |
| CWD     |           |             |
| DELE    |           |             |
| LIST    |           |             |
| M`KD    |           |             |
| PASS    |           |             |
| PASV    |           |             |
| PORT    |           |             |
| PWD     |           |             |
| QUIT    |           |             |
| RETR    |           |             |
| RMD     |           |             |
| RNTO    |           |             |
| STOR    |           |             |
| `USER   |           |             |

come funziona il servizio mail ?

2 protocolli x realizzare posta elettronica !

MTA: server mittente parla direttamnete al sever destinatario ! (a livello di comunicazione/connessione)

se il client dovesse mandare + mail, nella slide 21 si clicla nella parte da DAtA in poi