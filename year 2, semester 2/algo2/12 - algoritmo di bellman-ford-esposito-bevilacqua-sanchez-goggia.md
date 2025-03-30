---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-30T09:55
completed: false
---
# algoritmo di bellman-ford
>[!info] problema
>dato un grafo diretto e pesato $G$, in cui i pesi degli archi possono essere anche **negativi** e fissato un suo nodo $s$, vogliamo determinare il costo minimo dei cammini che conducono da $s$ a tutti gli altri nodi del grafo. (se non esiste un cammino verso un determinato nodo, il costo sarà considerato infinito)

>[!warning] ricordiamo !!
>se il grafo è un DAG, per trovare le distanze (minime) mi basta un sort topologico (anche se ha pesi negativi). abbiamo fatto un esercizio a riguardo !

>[!info] lessico
>se i costi degli archi possono essere negativi, non ha senso parlare di distanze, in quanto la distanza è una metrica (e come tale non può essere negativa). si parla quindi di costo dei cammini

## cicli negativi
si nota subito come, affichè esista un cammino negativo, non può esistere un **ciclo negativo**, cioè un ciclo diretto in un grafo in cui la soma dei pesi degli archi che lo compongono è negativa
>[!example]- esempio di ciclo negativo
![[Pasted image 20250330095306.png]]
> il ciclo evidenziato in figura è negativo, di costo -2

infatti, se in un cammino tra i nodi $s$ e $t$ è presente un nodo che appartiene ad un ciclo negativo $W$, allora non esiste un cammino minimo tra $s$ e $t$: ripassando più volte attraverso il ciclo $W$, possiamo abbassare arbitrariamente il costo del cammino da $s$ a $t$ !
affinchè esista un cammino minimo, non devono esistestere cicli negativi (percorro molteplici volte il ciclo negativo e il costo del cammino diminuisce all’infinito)




uso matrice $n-1 \cdot n-1$
se ho cicli negativi, la n-1-esima riga sarà diversa dall n-esima iga 

uso matrice pazza. riga 0: i cammini ottenibili con 0 archi
riga 1: cammini 

btw, ad ogni passo, a noi interessa solo la riga precedente ! non tutte le righe precedenti


slide 10
prendo l’arco minimo che va da un nodo x a j


i due for all’interno costano insieme $O(n+m)$
ciao