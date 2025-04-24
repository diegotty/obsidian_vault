---
related to: "[[13 - problemi di ottimizzazione, algoritmi di approssimazione]]"
created: 2025-03-02T17:41
updated: 2025-04-24T08:44
completed: false
---
## problema della selezione
>[!info] problema
data una lista $A$ di $n$ interi distinti ed un intero $k$, con $1\leq k\leq n$, vogliamo sapere quale elemento occuperebbe la posizione $k$ se il vettore venisse ordinato
>
> casi particolari:
>- per $k=1$ avremo il minimo di $A$
>- per $k=n$ avremo il massimo di $A$
>- per $k=\left[ \frac{n}{2} \right]$ avremo il mediano di $A$

>[!info] dumb algo
>un semplice algoritmo di complessità $O(n\log n)$ è il seguente:
>```python
>def selezione1(A, k):
>	A.sort()
>	return A[k-1]
>```
>e in caso di $k=1$ o $k=n$, il problema si riduce alla ricerca del minimo o del massimo, problemi risolvibili in $\Theta(n)$

>[!tip] vedremo, con la tecnica del **divide_et_impera** (cool formatting) che il problema può risolversi in $\Theta(n)$ anche nel caso generale
>dimostreremo quindi che il problema della selezione è computazionalmente più semplice di quello dell’ordinamento (che invece richiede tempo $\Omega(n\log n)$)
### approccio basato su divide et impera
```
scegli nella lista A l'elemento in posizione A[0] (che chiameremo perno)
a partire da A, costruisci due liste A_1 ed A_2, la prima contenente gli elementi di A minori del perno, e la secon
```