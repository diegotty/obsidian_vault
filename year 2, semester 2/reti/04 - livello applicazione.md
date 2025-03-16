---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-16T23:35
completed: false
---
# livello applicazione
come abbiamo visto nel [[03 - introduzione allo stack protocollare TCPslashIP#livello 5 applicazione|livello applicazione]], i protocolli in questo livello sono spesso parte di app
>[!info] evoluzione app Internet
![[Pasted image 20250316230932.png]]
## WWW
**WWW**(World Wide Web): applicazione Internet nata dall necessità di scambio e condivisione di informazioni tra ricercatori universitari di varie nazioni
>[!info] storia
>- 1990: inizialmente una rete di documenti collegati utilizzata sopratutto per scopi universitari
>- prototipo basato su testo (collegamenti ipertestuali)
>- 1993: primo browser grafio (mosaic - netscape)
>- 1994: **W3C**(World Wide Web Consortium):  organizzazione per sviluppare ulteriormente il Web, standardizzare i protocolli, etc.

caratteristiche:
- opera on demand (al contrario di approcci proattivi, in cui c’è sempre qualcosa in funzione)
- facile rendere informazioni disponibili
- **hyperlinks**(collegamenti ipertestuali) e motori di ricerca permettono di navigare su una grande quantità di siti
componenti:
- web client: interfaccia con l’utente (es: browser)
- web server: (es: apache)
- HTML: linguaggio standard per pagine web
- HTTP: protocollo per la comunicazione tra clienti e server Web (indipendente dal linguaggio di programmazione usato)

>[!info] terminologia
>- una pagina web è costituita da **oggetti**
>- ogni oggetto puo essere un file HTML, un’immagine, un’applet, un file audio, …
>- una pagina web è formata da un file base HTML, che include diversi oggetti referenziati (una richiesta da parte del client ad un server può scaricare richieste ad altri server
>- ogni oggetto è referenziato da un **URL**
## URL
gli oggetti rifererenziati si possono recuperare attraverso l’**URL**(Uniform Resource Locator), che è composto di 3 parti:
1. il protocollo
2. il nome della macchina in cui è situata la pagina
3. il percorso del file (localmente alla macchina), che indica la pagina, il nome del file e la posizione nel filesystem
```
protocol://host/path #utilizzato quando la porta è standard
protocol://host:porta/path #utilizzato quando è necessario specificare il numero di porta
```
## documenti web
- documento statico: contenuto predeterminato e memorizzato sul server
- documento dinamico: alcune delle informazioni vengono generate dal web server alla ricezione della richiesta (es: date)
- documento attivo: contiene script(es: applet java) o programmi che verranno eseguiti nel browser (ovvero lato client)
# HTTP
come abbiamo visto, il protocollo **HTTP**(hypertext transfer protocol) è uno dei protocolli di comunicazione usati per accedere ai documenti richiesti dall’utente (lato client)
- definisce quindi in che modo i client web richiedono le pagine ai server web, e come queseti le trasferiscono ai client
caratteristiche:
- si basa sul modello client/server
	- client: il browser, che richiede, riceve e visualizza gli oggetti del Web
	- server: il server web che invia oggetti in risposta a una richiesta
>[!example]- esempio di connessione HTTP
![[Pasted image 20250316232817.png]]

## connessioni HTTP
esistono due modalità per ricevere più oggetti dallo stesso server:
- connessioni non persistenti: viene stabilita una connessione TCP per ogni file. prima di inviare una richiesta al server è necessario stabilire una connessione
	- le richieste non persistenti si possono parallelizzare(tipicamente da 5 a 10 connessioni), quindi potenzialmente sono più veloci ma con più overhead
	- richiedono 2 RTT per oggetto
- connessioni persistenti (modalità di default): la connessione viene chiusa quando rimane inattiva per un lasso di tempo configurabile
	- le richieste persistenti sono sequenziali 
	- un solo RTT di connessione per tutti gli oggetto referenziati + un RTT per ogni oggetto ricevuto dal server
### RTT
**RTT**(round trip time): tempo impiegato da un **piccolo** pacchetto per andare da client a server e ritornare al client. include i ritardi di propagazione, di accodamento e di elaborazione del pacchetto
prestazioni per 

slide 19
2 crlf per far capire che gli header sono finiti


da ricordare come sono strutturate richieste/risposte 

# cookie
HTTP è un protocollo “**senza stato**”: una volta che il server ha servito un client, se ne dimentica (non mantiene informazioni sulle richieste fatte)
può essere però necessario tenere traccia dell’utente: ci sono molti casi ! ( x offrire navigazione più personalizzata)

gli indirizzi IP non sono adatti in quanto gli utenti possono lavorare su computer condivisi, e molti ISP

si cerca quindi di creare una sessione

la sessione è implicita nello scambio di informazioni

file cookie mantenuto sul sistema terminale !!! (dell’utente) e gestito dal browser dell’utente


quando l’utilizzzo di avvicina alla capacità della ret (intensità vicina al 100%), il ritardo si fa consistente (minuti)

inviare messaggi più piccoli migliora le prestazioni perchè il carico è minore