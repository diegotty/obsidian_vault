---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-07T22:15
completed: false
---
# DFS
>[!info] versione iterativa per matrice di adiacenza
visitiamo il nodo in DFS e teniamo traccia dei nodi già visitati (per i cicli, problema che non avevamo con gli alberi)
>```python
>## con lista di adiacenza
>def DFS(u,M):
>	###
>	n = len(M) ##O(1)
>	visitati = [0]*n  ##O(n)
>	DFSr(u, M, visitati)	
>	return [x for x in range(n) if visitati[x]] ##$\Theta(n)$
>	
>def DFsr(u, M, visitati):
>	visitati[u] = 1
>	for i in range(len(M)): ##$\Theta(n)$
>		if M[u][i] == 1 and not visitati[i]:
>			DFSr(i, M, visitati)
>```
>la complessità di questo algortimo è $O(n^2)$, in quanto il for-loop verrà eseguito al massimo $n$ volte
>- notiamo che nell’istruzione `return`, usiamo una lista, perchè l’inserimento in un set costa $O(n)$ (bisgona controllare se l’elemento può essere inserito), mentre l’inserimento in coda di una lista costa $\Theta(1)$
>- se però avessimo usato un insieme al posto della lista `visitati[i]`, la complessità dimensionale (che in questo caso è $O(n)$) sarebbe stata minore, e proporzionale ai nodi visitati


>[!info] versione iterativa per lista di adiacenza
stesso ragionamento dell’algoritmo per matrice di adiacenza
>```python
>## con lista di adiacenza
>def DFS(u,G):
>	###
>	n = len(G) ##O(1)
>	visitati = [0]*n  ##O(n)
>	DFSr(u, G, visitati)	
>	return [x for x in range(n) if visitati[x]] ##$\Theta(n)$
>	
>def DFsr(u, G, visitati):
>	visitati[u] = 1
>	for v in G[u]: ##$\Theta(n)$
>		if not visitati[v]:
>			DFSr(v, G, visitati)
>```
>la complessità di questo algortimo è $O(n +m)$, in quanto il for-loop verrà eseguito, in totale, $m$ volte (e se il grafo è diretto, saranno al massimo il doppio del massimo di un grafo indiretto)

>[!warning] dfs con lista binaria è $O(n+m)$
quindi c’è una differenza tra matrice binaria e lista di adiacenza ! sopratutto se il grafo è sparso ($m = O(n)$). ma anche se il grafo è completo (best case per la matrice binaria), le due visite sono uguali

>[!dimostrazione]- la correttezza dell’algorrimo per la visita DFS si dimostra con l’induzione
>(assumiamo che un nodo $i$ è raggiungibile e visitati[i] == 0)

>[!info] versione iterativa per lista di adiacenza
>```python
>def DFS_iterativo(u, G):
>	visitati = [0] * len(G)
>	pila = [u]
>	while pila:
>		u = pila.pop()
>		if not visitati[u]:
>			visitati[u] = 1
>			for v in G[u]:
>				if not visitati[v]:
>					pila.append(v)
>	return [x for x in range(len(G)) if visitati[x]]
>```
l’algoritmo ha complessità temporale $O(n+m)$, e complessità dimensionale $O(n)$
# albero DFS
con una visita DFS, gli archi del grafo si bipartiscono in:
- quelli che nel corso della visita sono stati attraversati
- quelli che nel corso della visita non sono stati attraversati
i nodi visitati e gli archi attraversati formano un albero detto **albero DFS**
>[!example] esempio
![[Pasted image 20250307221055.png]]
a sinistra un grafo $G$, a destra i tre alberi DFS che si ottengono facendo partire , rispettivamente, tre visite dai nodi 9, 4 e 3.
## vettore dei padri
(vettore dei **patri** in dialetto)
dato un albero DFS con $n$ nodi, esso si può memorizzare con un vettore $P$ di $n$ componenti:
- se $i$ è un nodo dell’albero DFS, $P[i]$ contiene il padre del nodo $i$ (per convenzione, il padre della radice è la radice stessa)
- se $i$ non è un nodo dell’albero, $P[i]$ per convenzione contiene $-1$
>[!example] esempi !
![[Pasted image 20250307221515.png]]