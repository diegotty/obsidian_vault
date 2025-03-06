---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-06T09:24
completed: false
---
# DFS
>[!info] versione iterativa
visitiamo il nodo
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
>	for i in range(len(M)):
>		if M[u][i] == 1 and not visitati[i]:
>			DFSr(i, M, visitati)
>```
>la complessità di questo algortimo è $O(n^2)$, in quanto il for-loop verrà eseguito al massimo $n$ volte
>- notiamo che nell’istruzione `return`, usiamo una lista, perchè l’inserimento in un set costa $O(n)$ (bisgona controllare se l’elemento può essere inserito), mentre l’inserimento in coda di una lista costa $\Theta(1)$


>[!warning] dfs con matrice binaria è $O(n+m)$
quindi c’è una differenza tra matrice binaria e lista di adiacenza ! sopratutto se il grafo è sparso ($m = O(n)$). ma anche se il grafo è completo (best case per la matrice binaria), le due visite sono uguali

>[!info] in-order, pre-order e post-order sono visite di alberi dfs !
## versione ricorsiva
dimostrazione x assurdo di DFS (assumiamo che un nodo è raggiungibile e visitati[] di quel nodo == 0)


usare un insieme al posto della lista visitati[i], la complessità dimensionale sarebbe minore e proporzionale ai nodi visitati