---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-20T12:24
completed: false
---
# algoritmo di dijkstra
>[!info] problema
> dato un grafo pesato, vogliamo troare i cammini minimi e quindi anche le distanze da un certo nodo $s$(sorgente) a tutti gli altri nodi del grafo
>**esempio:**
![[Pasted image 20250320111744.png]]

**l’algoritmo di dijkstra** è un algoritmo iterativo, che risolve il problema procedendo per passi(iterazioni) e prendendo una decisione ad ogni passo:
>[!info] logica dell’algoritmo
>costruisci l’albero dei cammini minimi un arco alla volta, partendo dal nodo sorgente:
>- ad ogni passo aggiungi all’albero l’arco che produce il nuovo cammino più economico (valutando tutti i possibili cammini da un nodo già visitato ad un nodo non visitato tramite un solo arco)
>- alla nuova destinazione assegna come distanza il costo del cammino
>
> a fine algoritmo, ogni nodo avrà la sua distanza minima dalla sorgente !
>- è quindi una visita, ma più intelligente di DFS e BFS, in quanto non prende sempre una decisione scelta a priori  

## tecnica greedy
l’algoritmo è il primo esempio che affrontiamo del **paradigma della tecnica greedy**:
- **si prendono una sequenza di decisioni irrevocabili**: ad ogni passo si decide il cammino dal nodo sorgente ad un nuovo nodo, e una volta deciso il cammino, non si può più tornare indietro sulla decisione !
- **le decisioni vengono prese in base ad un criterio “locale”**: tra tutti i nuovi cammini che si possono trovare estendendo i vecchi cammini di un arco, si sceglie quello che costa meno
>[!warning] svantaggi della tecnica greedy
l’applicazione di queste due regole porta ad algoritmi molto efficienti, infatti la tecnica greedy è infatti la prima tecnica che dovremmo provare ad applicare, di fronte ad un problema. 
>spesso però, produce una soluzione **subottimale**, in quanto non capita sempre che la scelta di una soluzione “locale” ottimale porti ad una soluzione globale ottimale
>- gli algoritmi greedy sono infatti spesso usati se non esistono algoritmi che trovano una soluzione corretta in tempo polinomiale, e si può dimostrare che l’algoritmo greedy ha un margine di errore piccolo

>[!tip] in questo caso l’algoritmo in porta alla soluzione **ottima** nei casi in cui l’algoritmo è corretto.
> infatti bisogna subito notare che l’algoritmo di dijkstra non è corretto nel caso di grafi con pesi anche negativi ! (può sbagliare, ma non è detto che sbagli sempre !)
## correttezza
dimostriamo la correttezza dell’algoritmo di dijkstra ! introduciamo lo pseudocodice dell’algoritmo:
```
Dijkstra(s, G):
- P[0...n-1] vettore dei padri inizializzato a -1
- D[0...n-1] vettore delle distanze inizializzato a +inf
- D[s], P[s] = 0, s
- while esistono archi {x, y} con P[x] != -1 e P[y] == -1:
	- sia {x,y} quello per cui è minimo D[x] + peso(x,y)
	- D[y], P[y] = D[x] + peso(x,y), x
return P, D
```
ad ogni iterazione del while viene assegnata una nuova distanza ad un nodo. per induzione sul numero di iterazioni, mostreremo che la distanza assegnata è quella minima
>[!dimostrazione] dimostrazione
caso base: $i=0$
>- i pesi sono non-negativi, quindi non esiste un cammino minimo tra $s$ ed $s$ minore di 0, di conseguenza il cammino minimo è 0 (non ci si muove)
>
>ipotesi induttiva: $i$
>- assumiamo che i cammini scelti dall’algoritmo per i primi $i$ passi siano cammini minimi
>
>passo induttivo: $i+1$
>- sia $T_{i}$ l’albero dei cammini minimi costruito fino al passo $i>0$, e sia $(u,v)$ l’arco aggiunto all’albero al passo $i+1$. faremo vedere che $D[v]$ è la distanza minima da $s$ a $v$. per fare ciò, basta mostrare che il costo di un eventuale cammino alternativo è sempre superiore o uguale a $D[v]$
>	- sia $C$ un qualsiasi cammino da $s$ a $v$ alternativo a quello presente nell’albero, e $(x,y)$ il primo arco che incontriamo percorrendo tale cammino $C$ all’indietro, tale che $x$ è nell’albero $T_{i}$ e $y$ no (tale arco deve esistere perchè $s$ è in $T_{i}$ mentre $v$ no). (prendiamo quindi in considerazione cammini che l’algoritmo non avrebbe potuto scegliere alla $i+1$-esima iterazione, in quanto $y$ non è stato ancora visitato.)
>	- per ipotesi induttiva, $\text{costo(C)} \geq \text{Dist(x) + peso(x,y)}$. (**questa affermazione è vera perchè i pesi del grafo sono tutti negativi**) (se non ci fosse stata una menzione del fatto che i pesi devono essere per forza positivi, la dimostrazione sarebbe stata sbagliata !)
>	- l’algoritmo ha preferito estendere l’albero $T_{i}$ con l’arco $(u,v)$ anzichè l’arco $(x,y)$, e in base alla regola con cui l’algorimo sceglie l’arco con cui estende l’ablero, deve quindi aversi: $D[x] + p(x,y) >= D[u]+p(u,v)$
>	- da cui segue che $\text{costo(C)} >= D[x] + \text{peso(x,y)} >= D[u] + p(u,v) = D[v]$
>	- quindi il cammino alternativo ha costo superiore a $D[v]$
![[Pasted image 20250320120024.png]]


## implementazione
>[!info] rappresentazione di grafi pesati
rappresenteremo il grafo pesato tramite lista di adiacenza, in cui: nella lista di adiacenza di $x$, invece di trovare solo il nodo di destinazione $y$, ci sarà la coppia $(y,c)$ dove $c$ è il peso dell’arco

>[!] implementazione con lista

```python
def dijkstra(s, G):
	n = len(G)
	lista = [0, float('inf'), -1] * n
	lista[s] = (1, 0, s)
	for (y, costo) in G[s]:
		lista[y] = (0, costo, s)
	while True:
		minimo, x = float('inf'), -1
		for i in range(n):
			if lista[i][0] == 0 and lista[i][1] < minimo:
				minimo, x = min(lista[i][1], i
		if minimo == float('inf'):
			break
		definitivo, costo_x, origine = lista[x]
		lista[x] = (1, costo_x, origine )


```
qualunque implementazione dell’algo di dijkstra è $\Omega(n+m)$ (in quanto devo arrivare a tutti i nodi e guardare tutti gli archi x forza)
implementazione facile/stupida è $O(n^2)$, senza uso di strutture dati particolari
- se il grafo non è sparso (m=$O(n^2)$), questa implementazione è ottima !
implementazione con heap : $O((n+m) \log n)$
- se il grafo è sparso, questa è la soluzione ottima ! se è denso, questa costa $O(n^2\log n)$ !!! quindi non ottima
terza implementazione: $O(n\log n + m)$ algo usato dal dijkstra di python, uitilizza un’heap particolare (heap di fibonacci) che ha prestazioni migliori

esiste un lower bound per dijkstra $\Omega(n\log^*n + m)$ ($\log^*n$ è il logaritmo iterato, cresce lentissimamente, è quasi una costante !) (la funzione di hackermann è l’inverso (huh, fatto da piperno))

in generale i grafi sono sparsi ! 
## rappresentazione di grafi pesati
ogni arco è rappresentato da un coppia (destinazione, costo)