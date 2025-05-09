---
related to: "[[03 - introduzione allo stack protocollare TCP-IP]]"
created: 2025-03-02T17:41
updated: 2025-05-09T22:23
completed: false
---
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
- **collegamento broadcast**: il collegamento è condivisio tra varie coppie di dispositivi (viene utilizzata solo una parte del mezzo)

inoltre un datagramma può essere gestito da diversi protocolli su collegamenti differenti ! e anche i servizi erogati dai protocolli del livello di link possono essere diversi (ad esempio, non tutti i protocolli forniscono un servizio di consegna affidabile)
## servizi offerti dal livello di collegamento
- **framing**: i protocolli incapsuano i datagrammi del livello di rete all’interno di un frame a livello di link, al fine di separare i vari messaggi durante la trasmissione da una sorgente ad una destinazione. in particolare, vengono utilizzati gli **indirizzi MAC** per identificare origine e destinatario
- **consegna affidabile**: la trasmissione è basata su `ACK` come nel livello di trasporto. è considerata non necessaria nei collegamenti che presentano un basso numero di errori sui bit (es: fibra ottica, cavo coassiale e doppino intrecciato) , ma è spesso utilizzata nei collegamenti soggetti ad elevati tassi di errori (es: collegamenti wireless)
- **controllo di flusso**: evita che il nodo trasmittente saturi quello ricevente (come in livello di rete)
- **rilevazione degli errori**: il nodo ricevente individua la presenza di errori, grazie all’inserimento da parte del nodo trasmittente di bit di controllo di errore all’interno del frame
- **correzione degli errori**: il nodo ricevente determina anche il punto in cui si è verificato l’errore, e lo corregge

>[!info] errori su singolo bit o a burst

il livello di collegamento è implementato all’interno della **network interface card** (**adattatore di rete**: la scheda ethernet, PCMCI), che implementa sia il livello di collegamento che il livello fisico
- la scheda di rete è una combinazione di hardware, software e firmare
>[!info] scheda di rete
![[Pasted image 20250509222356.png]]

il livello di collegamento può essere separato in due sottolivelli:
## data-link control
## media access control