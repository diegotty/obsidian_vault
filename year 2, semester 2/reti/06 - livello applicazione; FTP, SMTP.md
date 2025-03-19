---
related to: "[[03 - introduzione allo stack protocollare TCP-IP]]"
created: 2025-03-02T17:41
updated: 2025-03-19T11:12
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