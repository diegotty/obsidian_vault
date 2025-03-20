---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-20T11:40
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
>- i pesi sono non-negativi, quindi non esiste un cammino




dimostrazione: induzione sui passi
i = 0 : uso il fatto che i pesi sono non-negativi per sapere che il cammnimo ninimo tra la sorgente e la sorgente è 0
i-1 le distanze settate sono corrette( ipotesi ind)
i: passo induttivo: ipotiziamo per assurdo che esiste un cammino alternativo con distanza minore: questo ipotetico camino, ad un certo punto deve raggiungere $s$, la sorgente. e tale cammino non può costare meno di $f+c_{1}$. ma ciò è impossibile perchè dijkstra ha scelto $f+c_{1}$ xke costava di meno

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