---
related to: "[[13 - problemi di ottimizzazione, algoritmi di approssimazione]]"
created: 2025-03-02T17:41
updated: 2025-04-24T11:09
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
>[!info] logica
>- scegli nella lista $A$ l'elemento in posizione $A[0]$ (che chiameremo perno)
>- a partire da $A$, costruisci due liste $A_1$ ed $A_2$, la prima contenente gli elementi di $A$ minori del perno, e la seconda gli elementi di $A$ maggiori del perno
>dove si trova l'elemento di rango $k$ ?
>	- se $|A_1| \geq k$, allora l'elemento di rango $k$ è nel vettore $A_1$
>	- se $|A_1| = k-1$, allora l'elemento di rango $k$ è proprio il perno $x$
>	- se $|A_1| < k-1$, allora l'elemento di rango $k$ è in $A_2$, ed è l'elemento di rango $k-|A_1|-1$ in $A_2$

si nota come con questa logica, dopo aver costruito le due liste $A_{1}$ e $A_{2}$, grazie al test sulla cardinalità di $A_{1}$, il problema della selezione dell’elemento di rango $k$ o risulta risolto **oppure viene ricondotto alla selezione di un alemento in una lista con meno di $n$ elementi** (in quanto $A_{1}$ e $A_{2}$ non sono ordinate !!!)
>[!info] implementazione
>```python
>def selezione2(A, k):
>	if len(A) == 1:
>		return A[0]
>	perno = A[0]
>	A1, A2 = [], []
>	for i range(1, len(A)):
>		if A[i] < perno:
>			A1.append(A[i])
>		else:
>			A2.append(A[i])
>	if len(A1) >= k:
>		return selezione2(A1, k)
>	elif len(A1) == k-1:
>		return perno
>	return selezione2(A2, k - len(A1) - 1)
>```

la procedura tripartisce la lista in $A_{1}$, $A[0]$ ed $A_{2}$, e può restituire una partizione massimamente sbilanciata, in cui si ha, ad esempio, $|A_{1}| = 0$ e $|A_{2}| = n-1$. questo accade quando il perno risulta essere l’elemento minimo nella lista

qualora questo evento **sfortunato** si ripetesse sistematicamente nel corso delle varie partizione eseguite dall’algoritmo, che può succedere quando cerco il massimo in una lista ordinata, allora la complessità dell’algoritmo al caso pessimo viene catturata dalla sequente equazione:
$$
T(n)= T(n-1) + \Theta(n)
$$
(in quanto ad ogni iterazione impiego $\Theta(n)$)
che risolta dà
$$
T(n) = \Theta(n^2)
$$
in generale, la complessità superiore della procedura è catturata dalla ricorrenza 
$$
T(n) = T(m) + \Theta(n) \text{   , con  } m = max\{|A_{1}|, |A_{2}|\}
$$
>[!tip] fuoco
se avessimo una regola di scelta del perno in grado di garantire una partizione bilanciata, cioè $m=max\{|A_{1}|, |A_{2}|\} \approx \frac{n}{2}$
>allora per la complessità $T(n)$ dell’algoritmo, avremmo
>$$
>T(n) = T\left( \frac{n}{2} \right) + \Theta(n)
>$$
che risolta da $T(n) = \Theta(n)$

chiedere che la partizione sia perfettamente bilanciata è *forse* chiedere troppo. possiamo accontentarci di chiedere che la scelta del perno garantisca partizioni non troppo sbilanciate, ad esempio quelle per cui $m=max\{|A_{1}|, |A_{2}\} \approx \frac{3n}{4}$
in questo caso si ha
$$
T(n) \leq T\left( \frac{3}{4}n \right) + \Theta(n)
$$
che risolta da ancora $T(n) = \Theta(n)$
- in generale, finche $m$ è una frazione di $n$, anche piuttosto vicina ad $n$, come $\frac{99}{100}n$i, la ricorrenza da sempre $T(n) = \Theta(n)$
**IDEA**:
scegliamo il perno $p$ a caso in modo equiprobabile tra gli elementi della lista
- anche se la scelta casuale non produce necessariamente una partizione bilancaita, quanto visto ci fa intuire che la complessità rimane lineare in $n$