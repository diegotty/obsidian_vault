---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-09T19:26
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

>[!example] rete a circuito virtuale
![[Pasted image 20250409192031.png]]


il VC identifica il circuito tra mittente e destinazione


inoltrare ad una interfaccia significa inoltrare ad un router (ogni interfaccia porta ad un router)


tabella di routing PUÒ essere implementata su tutte le porte

### commutazione tramite bus


nelle porte di uscita si possono accodare i pacchetti perhce può capitare che più porte di input mandino pacchetti alla stessa porta di output

DHCP implementa funzioni di livello di rete, ma viene implementato a livello applicazione


identificatore, flag e offset usati per la frammentazione (che è una cosa che si può fare ceh è ok)


protocollo: ICMP< IGMP, OSPF usano tutti il protocollo IP anche se sono a livello di rete

