---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-02T18:08
---
# algoritmi efficienti
un algoritmo di dice **efficiente** se la sua complessità è di ordine polinomiale nella dimensione $n$ nell’input, **ovvero** di complessità $O(n^c)$ per una qualche costante $c$

di conseguenza, un algoritmo è **inefficiente** se la sua complesssità è di ordine **superpolinomiale**:
- **esponenziale**:  è una funzione di ordine $\Theta(c^n) = 2^{\Theta(n)}$
//TODO add why
- **super-esponenziale**: è una funzione che cresce più velocemente di un esponenziale, ad esempio $2^{\Theta(n^2)}$, ma anche $2^{\Theta(n\log n)}$
- **sub-esponenziale**: è una funzione che cresce più lentamente di un esponenziale, cioè $2^{O(n)}$
	- as esempio, $n^{\Theta(\log n)}=2^{\Theta(\log^2n)}$