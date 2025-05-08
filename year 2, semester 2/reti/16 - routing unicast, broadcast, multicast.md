---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-08T12:04
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
- **uncontrolled flooding**
- **controlled flooding**
>[!info] uncontrolled flooding
quando un nodo riceve un pacchetto broadcast, lo duplica elo invia a tutti i nodi vicini (eccetto a quello da cui lo ha ricevuto)
>- se il grafo ha cicli, una o più copie del pacchetto cicleranno all’infinito nella rete !!!! bad
![[Pasted image 20250508120111.png]]

>[!info] controlled flooding: sequence number
**NON** forwardare pacchetti già ricevuti e inoltrati: ogni nodo tiene una lista di (indirizzo IP, \#seq) dei pacchetti già ricevuti, duplicati, inoltrati
>- quando riceve un pacchetto, controlla nella lista: se già inoltrato lo scarta, altrimenti lo forwarda
>- per evitare i cicli dell’uncontrolled flooding !

>[!info] rever
