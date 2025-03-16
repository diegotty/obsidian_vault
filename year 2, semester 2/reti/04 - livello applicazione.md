---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-17T00:07
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
>[!example]- tempo di risposta
![[Pasted image 20250316233652.png]]
il tempo di risposta per una connessione è:
>- un RTT per iniziare la connessione TCP
>- un RTT per la richiesta HTTP e i primi byte della risposta HTTP
>- tempo di trasmissione del file
## messaggi HTTP
### richiesta HTTP
>[!info] richiesta HTTP
![[Pasted image 20250316233849.png]]
esempio di richiesta HTTP:
>```js
>GET /somedir/page.html HTTP/1.1 //request line
>Host: www.someschool.edu //header line
>Connection: close //header line
>User-agent: Mozilla/4.0 //header line
>Accept-Language:fr //header line
>(carriage return e line feed extra) //blank line
>```
#### metodi di richiesta

| metodo di richiesta | descrizione                                                                                                                                                                                           |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| GET                 | usato quando il client vuole scaricare, da un server, il documento specificato nell’URL. il server risponde con il documento richiesto nel corpo nel messaggio di risposta                            |
| HEAD                | usato quando il client non vuole scaricare il documento, ma solo alcune informazioni sul documento (es: data dell’ultima modifica). nella risposta, il server inserisce solo degli header informativi |
| POST                | usato per fornire input al server (es: contenuto dei campi di un form). l’input viene inserito nel corpo dell’identità                                                                                |
| PUT                 | utilizzato per memorizzare un documento nel server. il documento viene fornito nel corpo del messaggo, e la posizione di memorizzazione nell’URL                                                      |
>[!info] è posibile inviare info al server anche attraverso una richiesta GET
utilizzando gli `&` nell’url:
```js
GET www.somesite.com/animalsearch?monkeys&banana
```
#### header nella richiesta

| intestazione      | descrizione                                                           |
| ----------------- | --------------------------------------------------------------------- |
| User-Agent        | indicat il programma client utilizzato                                |
| Accept            | indica il formato dei contenuti che il client è in grado di accettare |
| Accept-charset    | famiglia di caratteri che il client è in grado di gestire             |
| Accept-encoding   | schema di codifica supportato dal client                              |
| Accept-language   | linguaggio preferito dal client                                       |
| Authorization     | indica le credenziali possedute dal client                            |
| Host              | host e numero di porta del client                                     |
| Date              | data e ora del messaggio                                              |
| Upgrade           | specifica il protocollo di comunicazione preferito                    |
| Cookie            | comunica il cookie al server                                          |
| If-Modified-Since | invia il documento solo se è più recente della data specificata       |
### risposta HTTP
>[!info] risposta HTTP
#### codici di risposta

| code | meaning      | examples |
| ---- | ------------ | -------- |
| 1xx  | information  |          |
| 2xx  | success      |          |
| 3xx  | redirection  |          |
| 4xx  | client error |          |
| 5xx  | server error |          |

#### intestazioni nella risposta

| intestazione     | descrizione                                               |
| ---------------- | --------------------------------------------------------- |
| date             | data corrente                                             |
| Upgrade          | specifica il protocollo preferito                         |
| Server           | indica il programma server utilizzato                     |
| Set-Cookie       | il server richiede al client di memorizzare un cookie     |
| Content-Encoding | specifica lo schema di codifca                            |
| Content-Language | specifica la lingua del documento                         |
| Content-Length   | indica la lunghezza del documento                         |
| Content-Type     | specifica la tipologia di contenuto                       |
| Location         | chiede al client di inviare la richiesta ad un altro sito |
| Last-Modified    | fornisce data e ora di ultima modifca del documento       |

# cookie
HTTP è un protocollo “**senza stato**”: una volta che il server ha servito un client, se ne dimentica (non mantiene informazioni sulle richieste fatte)
può essere però necessario tenere traccia dell’utente: ci sono molti casi ! ( x offrire navigazione più personalizzata)

gli indirizzi IP non sono adatti in quanto gli utenti possono lavorare su computer condivisi, e molti ISP

si cerca quindi di creare una sessione

la sessione è implicita nello scambio di informazioni

file cookie mantenuto sul sistema terminale !!! (dell’utente) e gestito dal browser dell’utente


quando l’utilizzzo di avvicina alla capacità della ret (intensità vicina al 100%), il ritardo si fa consistente (minuti)

inviare messaggi più piccoli migliora le prestazioni perchè il carico è minore