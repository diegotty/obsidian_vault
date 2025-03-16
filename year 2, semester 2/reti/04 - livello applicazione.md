---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-16T23:20
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
>- ogni oggetto puo essere un file HTML, un’imma
una richiesta da parte del client ad un server può scaricare richieste ad altri server (tipo collegamenti )

oggetti rifererenziati si possono recuperare attraverso l’URL

## documenti web
- documento statico
- documento dinamico: alcune delle informazion vengono generate dal web server alla ricezione della richiesta
- documento attivo: contiene script(es: applet java) o programmi che verranno eseguiti nel browser (ovvero lato client)

due modalità per ricevere più oggetti dallo stesso server:
- connessioni non persistenti: viene stabilita una connessione TCP per ogni file
- connessioni persistenti (modalità di default): la connessione viene chiusa quando rimane inattiva per un lasso di tempo configurabile

# HTTP
come abbiamo visto, il protocollo **HTTP** è uno dei protocolli di comunicazione usati per accedere ai document richiesti dall’utente (lato client)

prestazioni per 


richieste non persistenti: si possono paralelizzare quindi potenzialmente più veloci ma con più overhead
mentre in richieste persistenti le richieste sono sequenziali


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