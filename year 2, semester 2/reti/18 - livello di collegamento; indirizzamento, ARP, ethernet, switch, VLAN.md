---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-10T19:25
completed: false
---
# indirizzi MAC
anche chiamato indirizzo **fisico/ethernet/LAN**, è un indirizzo di 6 byte, che rappresenta **univocamente** ciascun **adattore** (scheda di rete) di una LAN
- quando un datagramma passa dal livello di rete al livello di collegamento, viene incapsulato in un frame con una intestazione che contiene gli **indirizzi di collegamento** (MAC) della sorgente e della destinazione del frame (non del datagramma !!)
>[!tip] differenza con indirizzi IP
gli indirizzi IP sono usati a livello di rete, e individuano con esattezza i punti di internet dove sono connessi gli host sorgente e destinazione

>[!info] indirizzi MAC
![[Pasted image 20250510184554.png]]

>[!info] come funziona il biz
![[Pasted image 20250510185415.png]]
notiamo che:
>- ogni stazione (adattatore/scheda di rete) ha un’indirizzo MAC
>- ogni router(/switch ?) ha un indirizzo MAC per ogni interfaccia (porta)
>- l’host inoltra il messaggio all’indirizzo MAC del prossimo hop, non all’indirizzo MAC del destinatario !
>	- ciò accade per ogni hop

la IEEE sovraintende la gestione degli indirizzi MAC:
- quando una società vuole costruire adattatori, compra un blocco di spazio di indirizzi (che sono unici e univoci)
con gli indirizzi MAC è possibile comunicare direttamente tra dispositivi (in una LAN, rete locale) a livello di collegamento, senza passare da un livello superiore
- è una comunicazione peer-to-peer, dove le stazioni utilizzano gli indirizzi MAC per comunicare
- è “orizzontale” perchè non prevede gerarchie: nessun router o server centrale è necessario !
- gli indirizzi IP invece hanno una struttura gerarchica, e deveono essere aggiornati se spostati (?)
# protocollo ARP
il protocollo **ARP** (**address resolution protocol**) permette ad una sorgente di determinare gli indirizzi di collegamento della destinazione
ogni **nodo IP**  (host, router) nella LAN ha una **tabella ARP**, che contiene la corrispondenza tra indirizzi IP e MAC. i suoi record sono del tipo
$$
<\text{indirizzo IP; indirizzo MAC; TTL}>
$$
in cui $\text{TTL}$ indica quando bisognerà eliminare una data voce nella tabella (il tempo di vita tipico è 20min.)
>[!example] esempio di tabella ARP
![[Pasted image 20250510190812.png]]

## procollo ARP nella stessa sottorete
>[!info] indirizzo mancante nella tabella ARP
![[Pasted image 20250510191003.png]]
>- $A$ vuole inviare un datagramma a $B$, e l’indirizzo MAC di $B$ non è nella tabella ARP di $A$
>- $A$ trasmette in un **pacchetto broadcast** il messaggio di richiesta ARP, contentente l’indirizzo IP di $B$
>	- l’indirizzo MAC del destinatario sarà `FF-FF-FF-FF-FF-FF` (indirizzo MAC di broadcast), e tutte le macchine della LAN riceveranno una richiesta ARP
> - solo il nodo con l’indirizzo IP specificato risponderà, comunicando ad $A$ il proprio indirizzo MAC
> 	- il frame viene inviato all’indirizzo MAC di $A$, in unicast
![[Pasted image 20250510191415.png]]

ARP è “plug-and-play”, in quanto la tabella ARP si costruisce automaticamente e non deve essere configurata dall’amministratore di sistema 
## formato del pacchetto ARP
>[!info]
>![[Pasted image 20250510191520.png]]
> - i protocol address sono indirizzi IP
> - gli hardware address sono indirizzi MAC

>[!example] esempio
![[Pasted image 20250510191545.png]]
## indirizzamento
l’indirizzamento è usato per inviare verso un nodo esterno alla sottorete
>[!info]- flusso di pacchetti dalla sorgente (riassunto corso ngl)
![[Pasted image 20250510191819.png]]
fuoco !!!!!!!!

>[!example] esempio
consideriamo la seguente richiesta HTTP da $A$ verso $B$, `http://dagabriele.biz/prodotti`
![[Pasted image 20250510191828.png]]
>
>il seguente è il flusso di pacchetti nella stazione sorgente: 
![[Pasted image 20250510192234.png]]
>
>il seguente è il flusso di pacchetti nel router $R1$:
![[Pasted image 20250510192122.png]]
>
>il seguente è il flusso di pacchetti nella stazione di destinazione:
![[Pasted image 20250510192432.png]]

## LAN cablate : ethernet