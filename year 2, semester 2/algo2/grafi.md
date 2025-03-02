---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-02T21:52
completed: false
---
# grafi
un grafo è rappresentato da $G(V,E)$, con: 
- $V=$ l’insieme dei nodi($|V|=n$) e 
- $E=$ l’insieme degli archi ($|E|=m$)

i grafi si distinguono in:
- **grafi diretti**: gli archi sono con direzione 
	- $0\leq m\leq n(n-1)=O(n^2)$
- **grafi indiretti**: gli archi non hanno direzione 
	- $0\leq m\leq \frac{n(n-1)}{2}=O(n^2)$

un grafo si dice **sparso** se $m=O(n)$,
altrimenti un grafo si dice denso se $m=\Omega(n^2)$
- inoltre un grafo si dice completo se ha tutt gli archi ($m=\Theta(n^2)$)
- un grafo diretto si dice **torneo** se tra ogni coppia di nodi c’è esattamente un arco ($m=\Theta(n^2)$)
>[!warning] un grafo non sparso non è necessariamente denso, ad esempio può avere $\Theta(n\log n)$ archi
## alberi
un albero è un grafo **connesso** senza cicli
- un albero ha sempre $m=n-1$ archi (è quindi un grafo sparso), e ciò si dimostra per induzione
## grafi planari
i grafi planari sono quei grafi che posso disegnare sul piano senza che gli archi si intersechino
>[!example] esempio di grafo planare
![[Pasted image 20250302213742.png]]

>[!warning] notare che gli alberi sono un **sottoinsieme** dei grafi planari !!!
![[Pasted image 20250302213839.png]]
### teorema di eulero
un grafo planare di $n>2$ nodi ha al più $3n-6$ archi.
- da ciò si può dedurre che da $n=5$ in poi esistono di certo grafi non planari (i grafi completi)
# rappresentazione di grafi
## matrici binarie
>[!info] immagine autoesplicativa
![[Pasted image 20250302214314.png]]
## liste di adiacenza
utilizzo una lista di liste $G$, che ha tanti elementi quanti sono i nodi del grafo $G$. $G[x]$ è una lista contenente i nodi **adiacenti** al nodo $x$ , vale a dire quelli raggiunti da archi che partono da $x$
- rispetto alla rappresentazione con matrice binaria, c’è un notevole risparmio di spazio nel caso di grafi sparsi, ma vedere se due archi sono connessi o meno può costare ora anche $O(n)$
>[!example] esempio
![[Pasted image 20250302214556.png]]
![[Pasted image 20250302214618.png]]

>[!exercise] esercizio
risolvere il problema del pozzo universale in tempo $O(n)$, avendo il grafo diretto rappresentato tramite una matrice di adiacenza