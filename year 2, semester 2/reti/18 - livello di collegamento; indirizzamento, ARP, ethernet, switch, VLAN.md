---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-10T20:27
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
nel 1985, la **IEEE computer society** (*nerds*) iniziò un progetto chiamato **progetto 802**, con l’obiettivo di definire uno standard per l’interconnessione tra dispositivi di produttori differenti
- lo scopo era di definire le funzioni del livello fisico e di collegamento dei protocolli LAN
il risultato fu il critically acclaimed and grammy nominated **standard IEEE 802**
## IEEE 802
raggruppa gli standard prodotti da IEE per le LAN. include: 
- specifiche generali del progetto (802.1)
- logical link control (LLC, 802.2)
- CSMA/CD (802.3)
- token bust (802.4, destinato a LAN per automazione industriale)
- token ring (802.5)
- DQDB (802.6, destinato alle MAN)
- WLAN (802.11)
i vari standard differiscono a livello fisico e nel [[17 - livello di collegamento#media access control|sottolivello MAC]], ma sono compatibili a livello **data link**
>[!info] rappresentazione
![[Pasted image 20250510194252.png]]

# ethernet
l’ethernet (cosa ? dammi una desrizione) detiene una posizione dominante nel mercato delle LAN cablate, ed è stata la prima LAN ad alta velocità, con vasta diffusione
- è più semplice e meno costosa di token ring, FDDI e ATM, ma rimane al passo dei tempi con il tasso trasmissivo: si va dall’ethernet standard (10mpbs) al 10 gigabit ethernet (10gbps)
>[!info] ethernet STANDARD
![[Pasted image 20250510194556.png]]
## ethernet standard
l’ethernet standard è:
- **senza connessione**: non è previsa alcuna forma di handshake preventiva con il destinatario prima di inviare un pacchetto
- **non afidabile** (come IP e UDP): la scheda di rete ricevente non invia un riscontro
>[!info] formato dei frame
![[Pasted image 20250510195137.png]]
> - **preambolo** (7 byte): tutti e 7 i byte hanno i bit `10101010`. serve per attivare le **NIC** (scheda di rete) dei riceventi e sincronizzare i loro orologi con quello del trasmittente, infatti fa parte dell’header del livello **fisico** !
> - **SFD** (**start frame delimiter**) (1 byte): di valore `10101011`, è il flag che definisce l’inizio del frame (è l’ultima possibilità di sincronizzazione, e gli ultimi due bit `11`indicano che inizia l’header MAC)
> - **indirizzi sorgente e destinazione** (6 byte): quando la scheda di rete riceve un pacchetto contenente il proprio indirizzo di destinazione o l’indirizzo broadcast(es: un pacchetto ARP), trasferisce il contenuto del campo dati del pacchetto al livello di rete, altrimenti i pacchetti con altri indirizzi MAC vengono ignorati
> - **tipo** (2 byte): per multiplexing/demultiplexing, indica il protocollo di livello rete del pacchetto incapsulato nel frame (IP, ARP, OSPF, etc.)
> - **dati** (da 46 a 1500 byte): contiene il datagramma di rete. se il datagramma ha dimensione minore della dimensione minima (46 byte), viene `zfill`- ato
> - **CRC** (4 byte): consente alla scheda di rete ricevente di rilevare la presenza di un errore nei bit sui campi di indirizzo, tipo e dati (idk how)
>
>contando i 18 byte di intestazione e trailer, la lunghezza minima del frame è di 64 byte  (necessaria per il corretto funzionamento del CSMA/CD), mentre la lunghezza massima del frame è di 1518 byte (necessaria per evitare che una stazione possa monopolizzare il mezzo, e per ragioni storiche (permetteva di ridurre la memoria necessaria nei buffer dei dispositivi, quando la memoria era molto costosa))

>[!tip] ovviamente
>tutte le stazioni che fanno parte di una ethernet sono dotate di NIC/scheda di rete, in quanto esse forniscono un indirizzo di livello di rete e livello di collegamento
> - gli indirizzi vengono trasmessi da sinistra verso destra, byte per byte, ma per ciascun byte il LSB viene inviato per primo e il MSB per ultimo

### fasi operative del protocollo CSMA/CD
1. **framing**: la NIC riceve un datagramma di rete dal nodo collegato e prepara un frame ethernet
2. **carrier sense e trasmissione**: se il canale è inattivo (viene misurato il livello di energia sul mezzo trasmissivo per un breve periodo di tempo, tipo 100 nanoscondi), inizia la trasmissione. se il canale risulta occupato, resta in attesa fino a quando non rileva più il segnale (e a quel punto trasmette)
3. **collision detection**: verifica, durante la trasmissione, la presenza di eventuali segnali provenienti da altre NIC. se non ne rileva, considera il pacchetto spedito
4. **jamming**: se rileva segnali da altre NIC, interrompe immediatamente la trasmissione del paccheto e invia un segnale di disturbo (fuck it, jam di 48bit per avvisare le altre NIC in fase di trasmissione della collisione)
5. **backoff esponenziale**: la NIC rimane in attesa. quando riscontra l’$n$-esima collisione consecutiva, stabilisce un valore $k$ tra $\{0, 1, 2, \dots,2^m-1\}$, dove $m = min(n, 10)$. la NIC aspetta un tempo pari a $k$ volte 512 bit, e ritorna al passo 2.
	- un tentativo per stimare quanti sono le NIC coinvolte (se sono numerosi, il tempo di attesa potrebbe essere lungo)

## fast ethernet
l’ethernet standard si è evoluta a **fast ethernet** (100mbps), mantenendo compatibilità con la versione precedente
in questa versione, il sottolivello MAC rimase invariato, compreso il formato del frame e le sue dimensioni, ma ci furono problemi con il funzionamento di CSMA/CD, in quanto il funzionamento dipende dalla velocità di trasmissione, dalla dimensione minima del frame, e dalla lunghezza massima della rete
- se la trasmissione è 10 volte più veloce, e il frame è ancora di 512 bit, allora le collisioni devono essere rilevate 10 volte più velocemente, quindi la rete deve essere 10 volte più corta !
### soluzioni per CSMA/CD
>[!info] prima soluzione
![[Pasted image 20250510202623.png]]
abbandonare la topologia a stella, e utilizzare un hub passivo con topologia a stella ma fissare la dimensione massima della rete a 250 metri, invece che 2500 metri della versione standard

>[!info]