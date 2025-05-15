---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-15T12:01
completed: false
---
abbiamo fatto:
es1
es2
es3 ?
esame ottobre 2020
esame settembre 2020
>[!example] problema
progettare un algoritmo che prende come parametro un intero $n$ e **stampa** tutte le stringhe binarie lunghe $n$
>>[!info] soluzione
>notiamo che le stringhe da stampare sono $2^n$, e che stampare una stringha lunga $n$ costa $\Theta(n)$ (se immaginiamo la stringa come una lista a cui aggiungere)
>quindi, per l’algoritmo che risolve questo problema, abbiamo $\Omega(n\cdot 2^n)$
>implementando una soluzione naive:
>>```python
>>def bin_stings(n, sol = []):
>>	if len(sol) == n:
>>		print(sol)
>>		return
>>	sol.append(1)
>>	bin_strings(n, sol)	
>>	sol.pop()
>>	sol.append(0)
>>	bin_strings(n, sol)
>>	sol.pop()
>>```
>>questa soluzione genera un albero di chiamate ricorsive alto $n$. l’albero ha quindi 
>>$$
>\sum_{i=0}^h = 2^i=2^{h+1}-1
>>$$
>nodi, che 
>ogni nodo esegue operazioni di costo $O(1)$, ma:
>>- ogni nodo interno richiede (effettivamente) tempo $O(1)$
>>- ciascuna foglia richiede tempo $O(n)$, in quanto deve stampare la stringa
>>dato che esistono $2^n$ nodi foglia, la complessità dell’algoritmo è 
>>$$
>>O(2^n) \cdot n + n \cdot O(1))= O(2^n\cdot n)+O(n)=O(2^n\cdot n)
>>$$

>[!example] problema (vincolo sugli zeri)
progettare un algoritmo che prende come parametro due interi $n$ e $k$, con $0\leq k\leq n$, e stampa tutte le stringhe binarie lunghe $n$ che contengono al più $k$ `1`

>[!info] soluzione
>è facile modificare la soluzione di sopra per ottenere una soluzione in $Theta(2^n \cdot n)$ (basta aggiungere una condizione che controlla il numero di `1`)
un buon algoritmo, però, dovrebbe avere una complessità proporzionale alle stringhe da stampare, vale a dire $O(\text{(stringhe da stampare)})$