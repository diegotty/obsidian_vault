---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-04T10:27
completed: false
---
# grafi
un grafo è rappresentato da $G(V,E)$, con: 
- $V=$ l’insieme dei nodi($|V|=n$) e 
- $E=$ l’insieme degli archi ($|E|=m$)

### grafi diretti e indiretti
i grafi si distinguono in:
- **grafi diretti**: gli archi sono con direzione 
	- $0\leq m\leq n(n-1)=O(n^2)$
- **grafi indiretti**: gli archi non hanno direzione 
	- $0\leq m\leq \frac{n(n-1)}{2}=O(n^2)$

### grafi sparsi e densi
un grafo si dice **sparso** se $m=O(n)$ (ha pochi archi),
altrimenti un grafo si dice denso se $m=\Omega(n^2)$
- inoltre un grafo si dice completo se ha tutt gli archi ($m=\Theta(n^2)$)
- un grafo diretto si dice **torneo** se tra ogni coppia di nodi c’è esattamente un arco ($m=\Theta(n^2)$)
>[!warning] un grafo non sparso non è necessariamente denso, ad esempio può avere $\Theta(n\log n)$ archi

**grado**(di un nodo): il numero di archi che incidono su un nodo
## alberi
un albero è un grafo **connesso**(ogni nodo è connesso agli altri) senza cicli
- un albero ha sempre $m=n-1$ archi (è quindi un grafo sparso), e ciò si dimostra per induzione
>[!dimostrazione]-  dim
induzione sul numero $n$ di nodi
>- c.b, $n=0$ → ci sono 0 archi
>- h.p → assumiamo che sia vero che per alberi con fino a $n-1$ nodi
>- passo induttivo → per un albero da $n$ nodi, mettendo da parte una foglia e l’arc che incide su di essa, rimane un albero di $n-1$ nodi. per ipotesi induttiva, tale albero avrà $n-2$ archi. aggiungendo l’arco che collega tale albero al nuovo nodo, si ottengon $n-1$ archi totali

si parla di **foglia** di un albero non radicato se un nodo ha un solo arco
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
>si nota che nei grafi indiretti, dato che ogni arco entrante è anche uscente, la rappresentazione è **simmetrica rispetto alla diagonale della matrice**
- uno dei problemi principali delle matrici binarie, è lo spreco di spazio (sopratutto se un grafo è sparso)
- uno dei vantaggi invece è la velocità con cui si può controllare la presenza di un arco: basta accedere all’elemento in posizione $(u,v)$, e ciò costa $O(1)$
## liste di adiacenza
utilizzo una lista di liste $G$, che ha tanti elementi quanti sono i nodi del grafo $G$. $G[x]$ è una lista contenente i nodi **adiacenti** al nodo $x$ , vale a dire quelli raggiunti da archi che partono da $x$
- rispetto alla rappresentazione con matrice binaria, c’è un notevole risparmio di spazio nel caso di grafi sparsi, ma vedere se due archi sono connessi o meno può costare ora anche $O(n)$
>[!example] esempio
![[Pasted image 20250302214556.png]]
![[Pasted image 20250302214618.png]]
- rispetto alla matrice binaria, il risparmio di spazio è notevole
- controllare la presenza di un arco può arrivare a costare $O(n)$, in quanto bisogna scorrere la lista dei nodi adiacenti al nodo $u$ per verificare $v$ sia presente (e anche se accedere a $G[u]$ è costante, la lista potrebbe contenere $n$ elementi !)

>[!exercise] esercizio
risolvere il problema del pozzo universale in tempo $O(n)$, avendo il grafo diretto rappresentato tramite matrice di adiacenza
un pozzo universale viene rappresentato in questo modo con una matrice di adiacenza:
![[Pasted image 20250304102416.png]]

notiamo che esiste un semplice test, con cui è possibile **sempre** eliminare uno dei nodi dai possibili pozzi universali:
$$
M[i][j] = \begin{cases}
1 & i \text{non è pozzo}
\end{cases}
$$

```python
pozzo_universale = false;
for i,list in G:
	if i != x:
		if x not in list:
			pozzo_universale = false;
		
```