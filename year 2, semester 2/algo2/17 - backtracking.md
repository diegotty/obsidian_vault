---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-15T12:16
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
un buon algoritmo, però, dovrebbe avere una complessità proporzionale alle stringhe da stampare, vale a dire $O(\text{(stringhe da stampare)}\cdot n)$
si calcola che le stringhe da stampare sono 
>$$
\sum_{i=0}^k \binom{n}{i} \approx n^{k+1}
>$$
introduciamo allora una **funzione di taglio**, che ci permette di **non** esplorare i rami degli alberi le cui fogli non ci interessano ! in modo da ottenere (per esempio) questo tipo di alberi
![[Pasted image 20250515120442.png]]
in questo caso, ci basta controllare il numero di `1` presenti nella stringa a mano a mano che aggiungiamo le cifre, in modo tale di evitare di non sprecare tempo in chiamate inutili

```python
def bin_string1(k,n, sol = []):
	if len(sol) == n:
		print(sol)
	if(k > 0):
		sol.append(1)
		bin_string(k-1, n, sol)
		sol.pop()
	sol.append(0)
	bin_string(k, n, sol)
	sol.pop()
```
testando i tempi di calcolo delle due funzioni, essi sono molto diversi: la soluzione naive impiega 18,1s, mentre `bin_string1()` impiega 0,002s. truly impressive !
la complessità dell’algoritmo è:
$n^k$

>[!warning] un modo semplice per calcolare il tempo dei propri **progammini** :)
usare la libreria `time` di pythons
## backtracking
si consideri un algoritmo di enumerazione basato sul **backtracking** dove l’albero di ricorsione ha altezza $h$, il costo di una foglia è $g(n)$ e il costo di un nodo interno è $f(n)$. se l’algoritmo gode della seguente proprietà:
$$
\text{un nodo viene generato solo se ha la possibilità di portare ad una foglia da stampare}
$$
allora la complessità dell’algoritmo è proporzionale al numero di cose da stampare $S(n)$. più precisamente, la complessità dell’algoritmo è:
$$
O(S(n)\cdot g(n) + S(n)\cdot h\cdot f(n))
$$
in quanto i nodi dell’albero che verranno effetivamente generati saranno $O(S(n)\cdot h)$, in quanto ogni nodo generato apparterrà ad un cammino che parte dalla radice e arriva ad una delle $S(n)$ foglie da enumerare (e worst case ogni foglia ha un cammino con tutti nodi diversi)
