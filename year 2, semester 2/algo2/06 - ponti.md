---
related to: "[[02 - visita DFS]]"
created: 2025-03-02T17:41
updated: 2025-03-15T20:53
completed: false
---
# ponti
un **ponte** è un arco la cui eliminazione disconnette il grafo
- i ponti rappresentano criticità del grafo ed è quindi utile identificarli

un grafo può non avere neanche un ponte così come avere tutti i suoi archi come ponti (come un albero !)
>[!example] grafi con ponti
![[Pasted image 20250313161103.png]]
>a sinistra un grafo senza ponti, a destra un grafo dove tutti gli archi sono ponti (albero)

>[!info] problema
dato un grafo connesso $G$, determinare l’insieme dei ponti del grafo

si può arrivare ad una prima soluzione usando la **ricerca esaustiva**: proviamo per ogni arco del grafo se esso è un ponte o meno
- verificare se un arco ${a,b}$ è un ponte per $G$ richiede $O(m)$ (basta eliminare l’arco e fare una ricerca DFS a partire da $a$ e vedere se si raggiunge $b$)
quindi, la complessità dell’algoritmo è:
$$
m\cdot O(m) = O(m^2) = O(n^4)
$$
in quanto $m = O(n^2)$ se il grafo non è sparso
>[!warning] è però possibile risolvere il problema in tempo lineare nella dimensione del grafo $O(m)$
basta infatti arrivare all’intuizione che devo controllare solo gli archi che vengono attraversati durante una visita ! in quanto sono quelli che bastano per un albero DFS e sono quelli che ci servono per visitare tutti i nodi ( gli altri sono “ridondanti”)
>- e ovviamente un arco non presente nell’albero DFS non può essere ponte, in quanto se lo elimino, gli archi dell’albero DFS continuano a garantire la connessione

quindi i ponti in un grafo non possono essere più di $n-1$ (numero di archi in un albero di $n$ nodi) (con questa ottimizzazione arriviamo a $O(n^3)$, ma si può fare molto di meglio !)

ci serve una caratterizzazione facile da calcolare, che ci consenta, durante la visita, di determinare se un arco dell’albero DFS è ponte o meno: notiamo che gli archi che non sono ponti sono **coperti** da archi non attraversati nella visita !

un nodo scopre se è ponte solo dopo che la visita di suo figlio è finita


fare procedura che individua archi di attraversamento(va da nodo a nodo già visitato e i due nodi non sono padre-figlio) e archi in avanti 

in grafi non diretti è impossibie trovare archi in avanti ! in quanto si troverebbe prima un arco all’indietro


durante una visita, ogni nodo può conoscere la sua altezza. ogni figlio gli comunica il nodo più in alto(con altezza più bassa) che è possibile raggiungere da lui o da un suo sottoalbero

se l’altezza del padre è minore del minimo raggiungibile, l’arco è un ponte


albero ha tutti punti di articolazione tranne le foglie ! 
# BFS
```python
def BFS(x, G):
	visitati = [0]*len(G)
	visitati[x] = 1
	coda = [x]
	while coda:
		u = coda.pop(0)
		for y in G[u]:
			if visitati[y] == 0:
				visitati[y] = 1
				coda.append(y) # y va in coda solo se non visitato
	return visitati	
```
 anche questa costa $O(n+m)$
 
 se implemento coda con lista, la complessità è :
 $$
 \Theta(n) + \Theta(1) + O(m) + O(n^2) = O(n^2)
 $$
 - usiamo BFS o DFS in base al problema che dobbiamo risolvere !
 