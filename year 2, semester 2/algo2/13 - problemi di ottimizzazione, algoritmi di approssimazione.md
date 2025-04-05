---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-05T17:40
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
>[!example] problema
> dato un grafo non diretto $G$, una sua **copertura tramite nodi** e un sottoinsieme $S$ dei suoni nodi tale che tutti gli archi di $G$ almeno un estremo in $S$