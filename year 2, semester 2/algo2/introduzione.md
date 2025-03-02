---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-02T20:18
---
lil ripasso
# theta-notation
$$\Theta(f(n))={g(n) | \exists k_{1},k_{2}, n_{0}t.c}$$
# algoritmi efficienti
un algoritmo di dice **efficiente** se la sua complessità è di ordine polinomiale nella dimensione $n$ nell’input, **ovvero** di complessità $O(n^c)$ per una qualche costante $c$

di conseguenza, un algoritmo è **inefficiente** se la sua complesssità è di ordine **superpolinomiale**:
- **esponenziale**:  è una funzione di ordine $\Theta(c^n) = 2^{\Theta(n)}$
//TODO add why
- **super-esponenziale**: è una funzione che cresce più velocemente di un esponenziale, ad esempio $2^{\Theta(n^2)}$, ma anche $2^{\Theta(n\log n)}$
- **sub-esponenziale**: è una funzione che cresce più lentamente di un esponenziale, cioè $2^{O(n)}$ (= $O(c^n)$)
	- as esempio, $n^{\Theta(\log n)}=2^{\Theta(\log^2n)}$ oppure $2^{\Theta(n^c)}$ dove $c$ è una costante inferioe ad 1.
## problemi sub-esponenziali
i problemi di cui si conoscono algoritmi sub-esponenziali e non polinomiali (quindi precisamente in mezzo), sono pochi e proprio per questo molto studiati.
- per esempio, per i problemi della fattorizzazione e dell’isomorfismo tra grafi, sono noti da tempo algoritmi superpolinomiali di complessità $2^O(n^{(1/3)})$ 
### test di primalità
l’algoritmo banale è esponenziale perchè ha complessità $O(n)$, e dato che la dimensione dell’input è $\log n$, l’algoritmo ha dimensione $O(2^{\log n})$
