---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-16T17:20
completed: false
---
# BFS
la **BFS** (breath-first-search) ci permette di visitare un albero con una logica diversa (e opposta) alla DFS:
la BFS esplora i nodi del grafo partendo da quelli a distanza 1 dalla sorgente $s$. poi visita quelli a distanza 2, e così via.
- l’algoritmo visita quindi tutti i vertici a livello $k$ prima di passare a quelli di livello $k+1$
>[!example] esempio di BFS
![[Pasted image 20250316171554.png]]

>[!info] logica dell’implementazione
per effettuare questo tipo di visita, manteniamo in una **coda** i nodi esaminati i cui adiacenti non sono stati ancora esaminati. ad ogni passo, preleviamo il primo nodo dalla coda, esaminiamo i suoi adiacenti e se scopriamo un nuovo nodo, lo aggiungiamo alla coda

>[!info] prima implementazione della BFS (naive)
>```python
>def BFS(x, G):
>	visitati = [0]*len(G)
>	visitati[x] = 1
>	coda = [x]
>	while coda:
>		u = coda.pop(0)
>		for y in G[u]:
>			if visitati[y] == 0:
>				visitati[y] = 1
>				coda.append(y) # y va in coda solo se non visitato
>	return visitati	
>```
la complessità di questo algoritmo è $O(n^2) 

## vettore delle distanze 
dati due nodi $a$ e $b$ di un grafo $G$, definiamo **distanza(minima)** in $G$ di $a$ da $b$ il numero minimo di archi che bisogna attraversare per raggiungere $b$ partendo da $a$.
- la distanza è posta a $+\infty$ se $b$ non è raggiungibile da $a$
>[!example]- distande di ogni nodo dal nodo $0$
![[Pasted image 20250316171248.png]]

il **vettore delle distanze** $D$ contiene quindi, in $D[y]$ la distanza di $y$ da $a$


 anche questa costa $O(n+m)$
 
 se implemento coda con lista, la complessità è :
 $$
 \Theta(n) + \Theta(1) + O(m) + O(n^2) = O(n^2)
 $$
 - usiamo BFS o DFS in base al problema che dobbiamo risolvere !