---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-20T11:25
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

## tecnica greedy
l’algoritmo è il primo esempio che affrontiamo del **paradigma della tecnica greedy**:
- **si prendono una sequenza di decisioni irrevocabili**: ad ogni passo si decide il cammino dal nodo sorgente ad un nuovo nodo, e una volta deciso il cammino, non si può più tornare indietro sulla decisione !
- **le decisioni vengono prese in base ad un criterio “locale”**: tra tutti i nuovi cammini che si possono trovare estendendo i vecchi cammini di un arco, si sceglie quello che costa meno

- ad ogni iterazione, calcola il prossimo passo in base al cammino minimo che porta ad una nuova destinazione

a fine algoritmo, ogni nodo avrà la sua distanza minima dalla sorgente

l’algorito di dijkstra è una visita + intelligente di DFS e BFS !

l’algoritmo è una semplice applicazione della tecnica greedy: ad ogni passo io prendo una decisione irrevocabile, in base ad una regola locale → algoritmi efficienti ! è infatti la prima tecnica che dovremmo provare ad applicare
- spesso però produce una soluzione subottimale, e spesso è usato se non esistono algoritmi che trovano la soluzione corretta in tempo polinomiale, e si può dimostrare che l’algoritmo greedy ha margine di errore piccolo

in questo caso l’algoritmo porta alla soluzione ottima !

dimostriamo che l’algoritmo è corretto
>[!warning] gli algoritmi greedy di solito non funzionano. lol

dijkstra non funziona nel caso in cui alcuni pesi degli alberi sono negativi ! (può sbagliare, non è detto che sbagli)

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