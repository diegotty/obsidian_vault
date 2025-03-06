---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-06T08:57
completed: false
---

```python
def DFS(u,M):
	###
	n = len(M)
	visitati = [0]*n
	DFSr(u, M, visitati)	
	return [x for x in range(n) if visitati[x]]
	
def DFsr(u, M, visitati):
	visitati[u] = 1
	for i in range(len(M)):
		if M[u][i] == 1 and not visitati[i]:
			DFSr(i, M, visitati)
```

dimostrazione x assurdo di DFS (assumiamo che un nodo è raggiungibile e visitati[] di quel nodo == 0)


esiste anche versione iterativa

dfs con matrice binaria è $O(n+m)$

quindi diff tra matrice binaria e lista di adiacenza !! sopratutto se il grafo è sparso (m = $O(n)$), ma anche se il grafo è completo, le due visite sono uguali

usare un insieme al posto della lista visitati[i], la complessità dimensionale sarebbe minore e proporzionale ai nodi visitati