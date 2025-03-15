---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-15T21:31
completed: false
---
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
 