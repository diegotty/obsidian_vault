---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-05T18:10
completed: false
---
# problemi di ottimizzazione
un **problema di ottimizzazione** è un tipo di problmea in cui l’obiettivo è trovare la **migliore** soluzione possibile tra un insieme di soluzioni ammissibili
- ogni soluzione ammissibile, cioè una soluzione che soddisfa tutte le condizioni imposte dal problema, ha un valore assoiato che può essere un “costo” o un “beneficio”, e a seconda del tipo del problema, l’obiettivo può essere minimizzare o massimizzare questo valore
>[!info] complessità dei problemi di ottimizzazione
sebbene esistono problemi di ottimizzazione per cui trovare la soluzione ottima è possibile in tempo polinomiale, nella maggior parte dei problemi di ottimizzazione trovare una soluzione ottima risulta essere un compito molto più difficile. in molti casi, trovare una soluzione ottima può esser un problema **NP-difficile**, dove la complessità cresce esponenzialmente con la dimensione del problema
>
>quindi,  sebbene determinare se una soluzione è ammissibile può essere fatto in tempo polinomiale, trovare la soluzione ottime richiede spesso algoritmi molto più complessi

## algoritmi di approssimazione
>[!info] problema della copertura tramite nodi
> dato un grafo non diretto $G$, una sua **copertura tramite nodi** e un sottoinsieme $S$ dei suoni nodi tale che tutti gli archi di $G$ almeno un estremo in $S$. trovare la copertura tramite nodi **di minima cardinalità**

>[!example] esempio di soluzione greedy non ottimale
un’approccio greedy sarebbe il seguente:
finchè ci sono archi non coperti, inserisci in $S$ il nodo che copre il **massimo** numero di archi ancora scoperti
>- ma questo algoritmo non produce sempre la soluzione ottimale. anzi, è molto facile “ingannarlo”
![[Pasted image 20250405174512.png]]

per questo problema (e moltissimi altri), definiti **computazionalmente difficili**, non si conoscono algoritmi neanche lontanamente efficienti (== non esistono algoritmi subesponenziali)
- in questi casi, potrebbe esser già soddisfacente ottenere una soluzione **ammissibile** che sia soltanto **”vicina”** ad una soluzione ottima, e ovviamente, più vicina e meglio è.
>[!warning] !!!!!!!
> fra gli algoritmi che non trovano sempre una soluzione ammissibile **ottima**, è importante distinguere due categorie piuttosto differenti: **algoritmi di approssimazione** ed **euristiche**
>- **gli algoritmi di approssimazione** sono algoritmi per cui si dimostra che la soluzione ammisibile prodotta approssima entro un certo grado una soluzione ottima !
>- le **euristiche**, invece, sono algoritmi per cui **non** si riesce a dimostrare che la soluzione ammissibile prodotta ha sempre una certa vicinanza ad una soluzione ottima. sono infatti l’ultima spiaggia, quando non si riesce a trovare nè algoritmi corretti nè algoritmi di approssimazione efficienti
>
>per una gran parte dei problemi computazionalmente difficili infatti, non si conoscono neanche buoni algoritmi di approssimazione. non è quindi sorprendente che fra tutti i tipi di algoritmi, gli algoritmi euristici costituiscano la classe più ampia e che ha dato luogo ad una letteratura **sterminata**

specifichiamo ora cosa si intende per “approssimazione entro un certo grado” (quando ci riferiamo ad algoritmi di approssimazione !)
>[!info] problemi di minimizzazione
>per i **minimizzazione**, dove ad ogni soluzione ammissibile è associato un costo, cerchiamo la soluzione ammissibile di costo minimo: il modo usuale di misurare il grado di approssimazione è il rapporto al **caso pessimo** tra il costo della soluzione prodotta dall’algoritmo e il costo della soluzione ottima.
>
formalmente: si dice che $A$ approssima il problema di minimizzazione entro un fattore di approssimazione $\rho$ se **per ogni istanza** $I$ del problema, vale:
>$$
>\frac{A(I)}{OPT(I)} \leq \rho
>$$
dove $OPT(I)$ è il costo di una soluzione ottime per l’istanza $I$, e $A(I)$ il costo della soluzione prodotta dall’algoritmo $A$ per quell’istanza

>[!info] problemi di massimizzazione
>per i problemi di massimizzazione, dove ad ogni soluzione ammissibile è associato un valore, si considera il rapporto inverso, ovvero:
>$$
\frac{>OTT(I)}{A(I)} \leq \rho
>$$

