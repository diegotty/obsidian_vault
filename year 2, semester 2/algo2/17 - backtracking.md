---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-15T16:58
completed: false
---
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
>>[!info] soluzione
>>è facile modificare la soluzione di sopra per ottenere una soluzione in $Theta(2^n \cdot n)$ (basta aggiungere una condizione che controlla il numero di `1`)
>un buon algoritmo, però, dovrebbe avere una complessità proporzionale alle stringhe da stampare, vale a dire $O(\text{(stringhe da stampare)}\cdot n)$
>si calcola che le stringhe da stampare sono 
>>$$
>\sum_{i=0}^k \binom{n}{i} \approx n^{k}
>>$$
>>introduciamo allora una **funzione di taglio**, che ci permette di **non** esplorare i rami degli alberi le cui fogli non ci interessano ! in modo da ottenere (per esempio) questo tipo di alberi
>>![[Pasted image 20250515120442.png]]
>>in questo caso, ci basta controllare il numero di `1` presenti nella stringa a mano a mano che aggiungiamo le cifre, in modo tale di evitare di non sprecare tempo in chiamate inutili
>>```python
>>def bin_string1(k,n, sol = []):
>>	if len(sol) == n:
>>		print(sol)
>>	if(k > 0):
>>		sol.append(1)
>>		bin_string(k-1, n, sol)
>>		sol.pop()
>>	sol.append(0)
>>	bin_string(k, n, sol)
>>	sol.pop()
>>```
>>testando i tempi di calcolo delle due funzioni, essi sono molto diversi: la soluzione naive impiega 18,1s, mentre `bin_string1()` impiega 0,002s. truly impressive !
>>la complessità dell’algoritmo è:
>>$$
>>n^{k} \cdot O(n) + n^{k+1}\cdot O(1) = O(n^{k+1}) + O(n^{k+1}) = O(n^{k+1})
>>$$

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
>[!example] problema (esame settembre 2020)
progettare un algoritmo che, dato un intero $n$, stampi tutte le stringhe di lunghezza $n$ sull’alfabeto dei simboli $\{a,b,c\}$ in cui il numero delle $b$ supera quello di ciascuno degli altri due simboli
>>[!info] soluzione
>l’algoritmo deve avere complessità proporzionale alle stringhe da stampare
>risolviamo l’esercizio allo stesso modo dell’ultimo esercizio precedente, ma teniamo traccia di 2 cose: il numero di $a$ e il numero di $c$ (in questo modo ci possiamo ricavare il numero di $b$). quindi rispettivamente `n_a` e `n_c`
>>
>>utilizziamo quindi una semplice funzione di taglio in cui:
>>- **inseriamo il simbolo $b$**: sempre, in quanto può sempre essere inserito
>>- **inseriamo il simbolo $a$**: solo se `n_a + 1 < n - (n_c + n_a + 1) && n_c < n - (n_c + n_a + 1)` 
>>- **inseriamo il simbolo $c$**:  solo se `n_c + 1 < n - (n_a + n_c + 1) && n_a < n - (n_c + n_a + 1)`
>>```python
>>def abc(n, n_a = 0, n_c = 0, sol = []):
>>	if len(sol) == n:
>>		print(sol)
>>		return
>>	sol.append(b)
>>	abc(n, n_a, n_c, sol)
>>	sol.pop()
>>	if n_a + 1 < n - (n_c + n_a + 1) and n_c < n - (n_c + n_a + 1):
>>		sol.append(a)
>>		abc(n, n_a + 1, n_c, sol)
>>		sol.pop()
>>	if n_c + 1 < n - (n_a + n_c + 1) and n_a < n - (n_a + n_c + 1):
>>		sol.append(c)
>>		abc(n, n_a, n_c + 1, sol)
>>		sol.pop()
>>```
>>l’albero prototto da questo algoritmo gode della proprietà che **un nodo viene generato solo se porta ad una foglia da stampare**, quindi la sua complessità è 
>>$$
O(D(n)\cdot h\cdot f(n) + D(n) \cdot g(n))
>>$$
>con
>>- $D(n)$ è il numero di stringhe da stampare
>>- $h = n$
>>- $f(n) = O(1)$
>>- $g(n) = O(n)$
>>quindi il costo di `abc()`  è $\Theta (n\cdot D(n))$


>[!example] problema
progettare un algoritmo che prende come parametro un intero $n$ e stampa tutte le matrici binarie $n \times n$

>[!info] soluzione
sappiamo che per un $2^{n \cdot n}$
```python
def all_matrixes():
	sol = [[0]*n for _ in range(n)]
	es1(n, sol)

def es1(n, sol, i = 0, j = 0):
	if i == n: # non n-1 !!!!!!
		for k in range(n):
			print(sol[k])
		print()
		return
	sol[i][j] = 0
	i1, j1 = i, j+1
	if j1 == n:
		i1, j1 = i+1, 0
	es1(n, sol, i1, j1)
	sol[i][j] = 1
	es1(n, sol, i1, j1)
```

l’algoritmo gode della proprietà per cui **ogni nodo viene generato se porta ad una matrice da stampare**,quindi la complessità dell’algoritmo è
$$
O(D(n)\cdot g(n)+ D(n)\cdot h\cdot f(n))
$$


>[!info] soluzione
- **inserisco un `1`**: sempre, in quanto posso sempre inserire un `1`
- **inserisco uno `0`**: posso inserire uno `0` nella cella `sol[i][j]` solo se sol`[i][j-1]` 