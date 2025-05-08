---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-08T12:34
completed: false
---
## definizioni
>[!info] unicast
**unicast**: comunicazione tra UNA sorgente e UNA destinazione
> - definita da: indirizzo IP sorgente - indirizzo IP destinazione
![[Pasted image 20250508115626.png]]

## broadcast
>[!info] broadcast 
>**broadcast**: invio di un pacchetto da un nodo sorgente a TUTTI i nodi della rete (quindi 1 a $N$, con $N$ = tutti i nodi della rete)
>- definita da: indirizzo IP sorgente - indirizzo broadcast di destinazione

il broadcast può essere eseguito in diversi modi: 
### uncontrolled flooding
>[!info] uncontrolled flooding
quando un nodo riceve un pacchetto broadcast, lo duplica elo invia a tutti i nodi vicini (eccetto a quello da cui lo ha ricevuto)
>- se il grafo ha cicli, una o più copie del pacchetto cicleranno all’infinito nella rete !!!! bad
![[Pasted image 20250508120111.png]]
### controlled flooding

>[!info] controlled flooding: sequence number
**NON** forwardare pacchetti già ricevuti e inoltrati: ogni nodo tiene una lista di (indirizzo IP, \#seq) dei pacchetti già ricevuti, duplicati, inoltrati
>- quando riceve un pacchetto, controlla nella lista: se già inoltrato lo scarta, altrimenti lo forwarda
>- per evitare i cicli dell’uncontrolled flooding !

>[!info] controlled flooding: reverse path forwarding (RPF)
forwarda il pacchetto solo se è arrivato dal link che e sul suo shortest path (unicast) verso la sorgente (quindi se è la prima volta che lo riceve)
>- in questo modo viene eliminato il problema dell’inondare la rete con troppi pacchetti, ma **NON** viene eliminata completamente la ritrasmissione di pacchetti ridondanti
>>[!example] esempio di RPF con pacchetti ridondanti
>>![[Pasted image 20250508120746.png]]

### spanning tree based
viene creato uno spanning tree (un grafo che include tutti i vertici originali ($V$), è un albero (connesso, no cicli), ed ha il numero miniore di archi per connettere tutti i vertici: $V-1$):
- si prende un nodo come centro: $E$
- ogni nodo invia un messaggio di join in unicast verso il centro, per costruire lo spanning tree (i messaggi vengono inoltrati finchè arrivano ad un nodo che appartiene all’albero oppure arrivano alla radice)
>[!example]- costruzionamento di spanning tree
![[Pasted image 20250508121205.png]]

per l’invio di messaggi in broadcast, i pacchetti vengono inoltrati solo sui link dello spanning tree, in modo tale che ogni nodo riceva solo una copia del pacchetto
## multicast
>[!info] multicast
comunicazoine tra una sorgente e **un gruppo** di destinazioni
![[Pasted image 20250508121354.png]]
>>[!tip] multicast come unicast multiplo
>>non è ottimale gestire il multicast come unicast multiplo, in quanto sarebbe inefficiente e aggiungerebbe ritardi (per il numero di pacchetti da inviare )
>>![[Pasted image 20250508121705.png]]

inoltre, molte applicazioni richiedono il trasferimento di pacchetti da uno o più mittenti ad un gruppo di destinatari, ad esempio:
- il trasferimento di un aggiornamento SW su un gruppo di macchine
- streaming audio/video ad un gruppo di utenti o studenti
- applicazioni con dati condivisi  (figjam (kelliot kelliot kelliot))
- aggiornamento di dati
### indirizzi multicast
nella pratica, sorge un problema banale: l’indirizzo di destinazione nell’IP può essere uno solo. per risolvere ciò, è necessario un **unico** indirizzo per tutto il gruppo, ovvero l’**indirizzo multicast**
>[!info] gruppo multicast
![[Pasted image 20250508122124.png]]
>- è importante ricordare che i router devono sapere quali host sono associati a un gruppo multicast !!
>	- deve quindi scoprire quali gruppi sono presenti in ciascuna delle sue interfacce per poi propagare le informazioni agli altri router
> - inoltre l’appartenenza ad un gruppo non è un attributo fisso dell’host (il periodo d’appartenenza può essere limitato)

viene quindi riservato un blocco di indirizzi per il multicast: in IPv4, è il blocco `224.0.0.0/4` ( da `224.0.0.0` a `239.255.255.255`), quindi $2^{28}$ indirizzi disponibili per gruppi multicast
>[!warning] un host che appartiene a un gruppo ha un indirizzo multicast separato e aggiuntivo rispetto al primario
>e l’indirizzo multicast non ha alcuna relazione con il prefisso associato alla rete

# IGMP
il protocollo **IGMP** (**internet group management protocol**) lavora tra un host e il router che gli è direttamente conesso, e offre agli host il mezzo per informare i router ad essi connessi del fatto che un’applicazione in esecuzione vuole aderire ad uno specifico gruppo multicast
- i messaggi vengono incapsulati in datagrammi IP, con IP protocol number 2 (huh ?), e `TTL` = 1
>[!info] rappresentazione
![[Pasted image 20250508122846.png]]
## messaggi IGMP
- **membership query**: mandati dal router all’host, per determinare a quali gruppi hanno aderito gli host su ogni interfaccia. vengono inviati periodicamente
- **membership report**: mandati dall’host al router, per informare il router su un’adesione (sia per rispondere ad una membership query che non  (quindi al momento dell’adesione))
- **leave group**: mandati dall’host al router, p