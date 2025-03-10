---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-10T09:55
completed: false
---
>[!warning] esattamente ciò che abbiamo fatto a [[26 - lock binario, lock a 2 fasi#ordinamento topologico|bd1]] ! 

>[!example] esempio problema
>abbiamo un lavoro da compiere, che è diviso in $n$ compiti (un nodo per ogni compito). tra certe coppie di compiti esiste una dipendenza: in particolare, un arco indica che per la coppia $(a,b)$, l’esecuzione del compito $b$ richiede la terminazione del compito $a$.
vogliamo sapere se è possibile completare il lavoro e, nel caso la risposta sia positiva, vogliamo un ordinamento con cui eseguire i compiti in modo da rispettare le dipendenze.
# ordinamento topologico
notiamo quindi che un grafo diretto può catturare relazioni di propedeuticità, e potrò rispettare tutte le propedeuticità se riesco ad ordinare i nodi del grafo in modo che gli archi vadano tutti da sinistra verso destra (in generale, in una sola direzione). questo ordinamento è detto **ordinamento topologico** !
>[!figure] ordinamento topologico
![[Pasted image 20250310095216.png]]

un grafo diretto può avere da $0$ fino ad $n!$ ordinamenti topologici, ma un algoritmo **esaustivo** (che genera sistematicamente i differenti ordinamenti dei nodi del grafo, e per ciascuno testa se è topologico) ha complessità $\Omega (n!)$, ed è quindi improponibile.
usiamo quindi un’altra strada per testare se il lavoro è completabile
## DAG
**DAG**: direct acyclic graph (grafo diretto aciclico)