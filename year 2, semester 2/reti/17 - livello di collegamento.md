---
related to: "[[03 - introduzione allo stack protocollare TCP-IP]]"
created: 2025-03-02T17:41
updated: 2025-05-09T22:53
completed: false
---
# livello di collegamento
la comunicazione a livello di collegamento è **hop-to-hop**
- host e router vengono chiamati **nodi** o **stazioni**
- i canali di comunicazione che collegano nodi adiacenti sono i **link**, e possono essere cablati o wireless
- le unita di dati scambiate dai protocolli a livello di link sono chimate **frame**
>[!info] rappresentazione
![[Pasted image 20250509221200.png]]

i protocolli a livello collegamento si occupano quindi del traporto di datagrammi lungo un canale di comunicazione (fisico o wireless)
### tipi di collegamento
esistono 2 tipi di link, a seconda della funzione che svolgono:
- **collegamento punto-punto**: dedicato a due soli dispositivi (viene utilizzata l’intera capacità del mezzo)
	- viene impiegato per connessioni telefoniche o tra ethernet e host
	- viene utilizzzto il **point-to-point protocol (PPP)**, del [[#data-link control|DLC]]
- **collegamento broadcast**: il collegamento è condivisio tra varie coppie di dispositivi (viene utilizzata solo una parte del mezzo)
	- include l’ethernet tradizionale (cavo/canale condiviso), o le LAN wireless 802.11
	- necessita di un protocolllo (il protocollo MAC) per la gestione del canale condiviso

inoltre un datagramma può essere gestito da diversi protocolli su collegamenti differenti ! e anche i servizi erogati dai protocolli del livello di link possono essere diversi (ad esempio, non tutti i protocolli forniscono un servizio di consegna affidabile)
## servizi offerti dal livello di collegamento
- **framing**: i protocolli incapsuano i datagrammi del livello di rete all’interno di un frame a livello di link, al fine di separare i vari messaggi durante la trasmissione da una sorgente ad una destinazione. in particolare, vengono utilizzati gli **indirizzi MAC** per identificare origine e destinatario
- **consegna affidabile**: la trasmissione è basata su `ACK` come nel livello di trasporto. è considerata non necessaria nei collegamenti che presentano un basso numero di errori sui bit (es: fibra ottica, cavo coassiale e doppino intrecciato) , ma è spesso utilizzata nei collegamenti soggetti ad elevati tassi di errori (es: collegamenti wireless)
- **controllo di flusso**: evita che il nodo trasmittente saturi quello ricevente (come in livello di rete)
- **rilevazione degli errori**: il nodo ricevente individua la presenza di errori, grazie all’inserimento da parte del nodo trasmittente di bit di controllo di errore all’interno del frame
- **correzione degli errori**: il nodo ricevente determina anche il punto in cui si è verificato l’errore, e lo corregge
## errori
gli errori sono dovuti a **interferenze**, che possono cambiare la forma del segnale.
esistono 2 tipi di errori: 
- **errori sul singolo bit**
- **errori burst**
normalmente la durata dell’interferenza è più lunga rispetto a quella di un solo bit, quindi gli errori burst sono più comuni rispetto a quelli sul singolo bit
- il numero di bit coinvolti dipende dalla velocità di trasferimento dati e dalla durata dell’interferenza (es: 1kpbs con rumore di 1/100sec può influire su 10bit !)
>[!info]- errori su singolo bit o a burst
![[Pasted image 20250509223228.png]]
![[Pasted image 20250509223239.png]]
### tecniche di rilevazione degli errori
la rilevazione degli errori non è attendibile al 100%: è possibile che ci siano errori non rilevati, ma per ridurre questa possibilità, le tecniche più sofisticate prevedono un’elevata ridondanza
>[!info] schema
![[Pasted image 20250509223459.png]]
>- **EDC**: error detection and correction
>- **D**: dati che devono essere protetti da errori, ai quali vengono aggiungi dei bit di EDC

#### controllo di parità
viene aggiungo un bit **di parità**, selezionato in modo da rendere pari il numero totale di 1 all’interno della codeword (payload ?)
ciò può essere fatto in 2 modi:
- unico bit di parità: ci permette di rilevare se si è verificato almeno un errore in un bit
- parità bidimensionale: ci permette di individuare e correggere il bit alterato
>[!info] rappresentazione
![[Pasted image 20250509223812.png]]
## implementazione del livello di collegamento
il livello di collegamento è implementato all’interno della **network interface card** (**adattatore di rete**: la scheda ethernet, PCMCI), che implementa sia il livello di collegamento che il livello fisico
- la scheda di rete è una combinazione di hardware, software e firmare
>[!info] scheda di rete
![[Pasted image 20250509222356.png]]

>[!info] esempio
![[Pasted image 20250509222454.png]]
durante la comunicazione:
>- il lato mittente:
>	- incapsula un datagramma in un frame
>	- imposta i bit rilevazione degli errori, trasferimento dati affidabile, controllo di flusso, etc.
>- il lato ricevente:
>	- individua errori, trasferimento dati affidabile, controllo di flusso, etc.
>	- estrae i datagrammi e li passa al nodo ricevente

il livello di collegamento può essere separato in due sottolivelli:
### data-link control
il **DLC** si occupa di tutte le questioni **comuni** sia ai collegamenti punto-punto che ai collegamenti broadcast:
- framing
- controllo del flusso e degli errori
- rilevamento e correzione degli errori
- procedure per la comunicazione tra 2 nodi adiacenti, indipendentemente dal fatto che il collegamento sia dedicato o broadcast
### media access control
il **MAC** si occupa solo degli aspetti specifici dei canali broadcast:
- controllo dell’accesso al mezzo condiviso
>[!warning] il livello di collegamento di un link punto-punto non ha il sotto-livello MAC !

## protocolli di accesso multiplo
quando viene usato un canale broadcast condiviso, è possibile che centinaia/migliaia di nodi possano comunicare direttamente sul canale broadcast. non è quindi raro che si generi una **collisione**, che accade quando i nodi ricevono due o più frame contemporaneamente
lo scopo dei protocolli di accesso multiplo è quello di evitare caos, e realizzare una condivisione. per fare ciò, fissano le modalità con cui i nodi regolano le loro trasmissioni sul canale condiviso
- la comunicazione relativa al canale condiviso deve utilizzare il canale stesso ! (non c’è un canale “out-of-band” per la coordinazione)
>[!info] protocolli di accesso multiplo ideali
idealmente, dato un canale broadcast con velocità $R$ bps:
>- quando un solo nodo deve inviare dati , esso dispone di un tasso trasmissivo pari a $R$ bps
>- quando $M$ nodi devono inviare dati, questi dispongono di un tasso trasmissivo pari a $\frac{R}{M}$ bps
>- ha un protocollo decentralizzato: non ci sono nodi master, o sincronizzazione dei clock

i protocolli di accesso multiplo si possono dividere in 3 categorie:
- **channel partitioning** (protocolli a suddivisione del canale):
- **random access** (protocolli ad accesso casuale):
- **taking-turn**: (protocolli a rotazione):