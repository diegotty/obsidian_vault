---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-10T08:24
completed: false
---
abbiamo visto, durante una prima visione della pila TCP/IP, che il livello di rete si occupa dell’instradamento dei datagrammi dall’origine al destinatario. approfondiamo !
>[!example] esempio di funzionamento del livello rete
![[Pasted image 20250409185452.png]]
>- il livello di rete di H1 prende i segmenti dal livello di trasporto, li incapsula in un datagramma, e li trasmette al router più vicino
>- il livello di rete di H2 riceve i datagrammi da R7, estrae i segmenti e li consegna al livello di trasporto
>- il livello di rete dei nodi intermedi inoltra verso il prossimo router
## funzioni chiave del livello di rete
il livello di rete si occupa di capire, quando un router riceve un pacchetto, a chi inviarlo per farlo arrivare al destinatario. le sue funzioni chiave sono 2:
- **instradamento** (routing): determina il percorso seguito dai pachetti dall’origine alla destinazione (costruisce le rotte)
- **inoltro** (forwarding): decide su quale porta di uscita deve essere immesso il pacchetto in base a quanto stabilito dal routing (utilizza il percorso definito dal routing)

>[!info] routing e forwarding
![[Pasted image 20250409190036.png]]
>le **forwarding table** vengono quindi generate dal **routing algorithm**, e vengono consultate per capire a chi inoltrare il pacchetto
>- la porta di uscita viene scelta in base all’IP di destinazione
# switching
esistono 2 approcci per lo **switching** (word that sounds like a sexual practice)
## reti a circuito virtuale
nell’approccio a **circuito virtuale**, prima che i datagrammi fluiscano, i due sistemi terminali e i router intermedi stabiliscono una connessione virtuale
un circuito virtuale consiste in:
1. un percorso tra gli host di origine e destinazione
2. numeri VC, uno per ciascun collegamento
3. righe nella tabella d’inoltro in ciascun router
ogni pacchetto di un circuito virtuale ha un **numero VC** (chiamata anche etichetta di circuito) nella propria intestazione, ed ogni router sostituisce il numero VC con un nuovo numero.

>[!info] tabella d’inoltro
![[Pasted image 20250410074142.png]]
oltre a questi campi, i router mantengono le informazioni sullo stato delle connessioni ! aggiungono alla tabella una nuova riga ogni volta che stabiliscono una nuova connessione, e la cancellano quando viene rilasciata //connessione tra terminali o tra 2 router ?

>[!example] rete a circuito virtuale
![[Pasted image 20250409192031.png]]
### ATM (lore)
la **ATM** (asynchronous transfer mode) è una rete orientata alla connessione progettata nei primi anni 90, con lo scopo di unificare voce, dati, televisione via cavo, etc.
- viene attualmente usata nella rete telefonica per trasportare (internamente) i pacchetti IP
- le connessioni vengono chiamate **circuiti virtuali** (in analogia con quelli telefonoci, che sono circuiti fisici)

## reti a datagramma
le reti a datagramma **non** sono orientate alla connessione, ed ogni datagramma viaggia indipendente dagli altri
- **Internet** è una rete a datagramma !
non esiste un concetto di “connessione” a livello di rete, ed i router non conservano informazioni sullo stato dei circuiti virtuali
- i pacchetti vengono inoltrati utilizzando l’indirizzo dell’host destinatario, e passano attraverso una serie di router, che utilizzano gli indirizzi di destinazione per inviarli e possono intraprendere percorsi diversi !
>[!info]- tabella di inoltro
![[Pasted image 20250410075354.png]]
la tabella di inoltro è gestita creando dei bucket per gli indirizzi (che sappiamo avere dimensione di 4 byte), con questa logica:
![[Pasted image 20250410075515.png]]
ma in modo più efficiente: confrontando il prefisso dell’indirizzo:
![[Pasted image 20250410080138.png]]
>- se si verificano corrispondenze multiple, si prende la corrispondenza a prefisso più lungo

>[!example] rete a datagramma
![[Pasted image 20250410075303.png]]
>i pacchetti possono arrivare fuori ordine !
## switch e router
**packet switch** (commutatore di pacchetto): dispositivo che si occupa del trasferimento dall’interfaccia di ingresso a quella di uscita, in base al valore del campo dell’intestazione del pacchetto.
noi studieremo (per ora ? ) 2 tipi di packet switch:
- **router**: stabiliscono l’inoltro in base al valore del campo (indirizzo IP) nel livello di rete (livello 3)
- **link-layer switch** (commutatore a livello di collegamento): stabiliscono l’inoltro in relazione al valore del campo (indirizzo MAC) nel livello di collegamento (link / livello2)
>[!example] esempio !
![[Pasted image 20250409191012.png]]
>immagino che un link-layer switch sia più veloce (in quanto deve risalire meno pila TCP/IP) 

### link-layer switch
viene utilizzato per collegare singoli computer all’interno di una rete LAN, ed instrada pacchetti al livello 2
>[!info] idk what this image is trying to depict but definitely aesthetic
![[Pasted image 20250409191220.png]]
>in pratica è un switch collegato ad un ethernet e tanti host (?)
### router
instrada i pacchetti al livello 3, da uno dei suoi link entranti ad uno dei suoi link uscenti: al **next hop** nel percorso origine-destinazione
>[!info] autoesplicativo
![[Pasted image 20250409191709.png]]

studiamo ora cosa si trova all’interno del router: 
>[!info] architettura del router
![[Pasted image 20250410080457.png]]

>[!info] porte d’ingresso
![[Pasted image 20250410081344.png]]
la **commutazione decentralizzata** determina la porta d’uscita dei pacchetti utilizzando le informazioni della tabella d’inoltro (**ogni porta d’ingresso ha una copia della tabella !**)
>- l’obiettivo è quello di completare l’elaborazione allo stesso **tasso** della linea (evitando quindi colli di bottiglia). l’accodamento accade se il tasso di arrivo dei datagrammi è superiore a quello di inoltro !
>una volta determinata la porta di uscita, il pacchetto verrà inoltrato alla struttura di commutazione

la ricerca nella tabella di inoltro deve essere quindi veloce (sempre per evitare accodamenti). implementa una **struttura ad albero**: ogni livello dell’albero corrisponde ad un bit dell’indirizzo di destinazione
- per cercare un indirizzo si comincia quindi dalla radice e si sceglie sottoramo sinistro se il bit è 0 e destra se il bit è 1. la ricerca finirà in $N$ passi, con $N$ il numero di bit dell’indirizzo
>[!warning] gli IP address lookup algorithms è un argomento molto studiato ! 
### tecniche di commutazione
studiamo ora i modi per far attraversare un pacchetto dalla porta di ingresso alla porta di uscita:
>[!info] commutazione in memoria
utilizzata dall prima generazione di router: il pacchetto veniva copiato nella memoria del processore, e veniva trasferito dalla porta d’ingresso alla porta d’uscita
![[Pasted image 20250410082409.png]]


nelle porte di uscita si possono accodare i pacchetti perhce può capitare che più porte di input mandino pacchetti alla stessa porta di output

DHCP implementa funzioni di livello di rete, ma viene implementato a livello applicazione


identificatore, flag e offset usati per la frammentazione (che è una cosa che si può fare ceh è ok)


protocollo: ICMP< IGMP, OSPF usano tutti il protocollo IP anche se sono a livello di rete

