---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-08T12:19
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