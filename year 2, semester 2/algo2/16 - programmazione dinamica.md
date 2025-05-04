---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-04T12:25
completed: false
---
sappiamo che gli algoritmi basati sulla tecnica divide_et_impera seguono 3 passi:
1. dividi il problema in sottoproblemi di taglia inferiore
2. risolvi (ricorsivamente) i sottoproblemi di taglia inferiore
3. combina le soluzioni dei sottoproblemi in una soluzione del problema originale
>[!tip]
negli esempi visti finora, i sottoproblemi che si ottenevano dall’applicazione del passo 1 erano tutti **diversi**, pertanto ciascuno di essi veniva individualmente risolto dalla relativa chiamata ricorsiva del passo 2. in molte situazioni però, i sottoproblemi ottenuti al passo 1 possono risultare **uguali**. in tal caso, l’algoritmo basasto sulla tecnica *divide_et_impera* **risolve più volte lo stesso problema, svolgendo lavoro inutile**

>[!example] esempio : er fibonacci
scriviamo un algoritmo che, dato un intero $n$, calcola $f_{n}$ (con $f_{i} = f_{i-1}+f_{i_{2}}$, e $f_{0}=f_{1} = 1$)
>
il primo algoritmo che viene in mente per risolvere il problema è basato sul *divide_et_impera* e sfrutta la definizione di numero di fibonacci:
>```python
>def Fib(n):
>	if n <= 1: return 1
>	a = Fib(n-1)
>	b = Fib(n-2)
>	return a + b
>```
>l’equazione di ricorrenza per il tempo di calcolo dell’algoritmo è: 
>$$
>T(n) = T(n-1) +  T(n-2) + O(1)
>$$
>dove ovviamente
>$$
>T(n) \geq 2T(n-2) + O(1)
>$$
>che, risolta, produce $T(n)\geq \Theta(2^{\frac{n}{2}})$

>[!info] il problema
>il motivo di tale inefficienta sta nel fatto che il programma `Fib(n)` viene chiamato più volte sullo stesso input
>![[Pasted image 20250504111025.png]]
i sottoproblemi in cui viene scomposto il problema **non sono disgiunti**: questo fenomeno prende il nome di **overlapping di sottoproblemi**.
>- ma l’algoritmo di somporta come se fossero nuovi, e li calcola nuovamente
>
>per risolvere l’overlapping, basta memorizzare in una lista i valori $fib(i)$ quando li si calcola per la prima volta, cosicchè nelle chiamate ricorsive a $fib(i)$ non ci sarà più bisogno di ricalcolarli  ma potranno essere ricavati dalla lista. stiamo applicando quindi la tecnica della **memoizzazione**
>```python
>def Fib(n):
>	F = [-1]*(n+1)
>	return memFib(n, F)
>
>def memFib(n, F):
>	if n <= 1: return 1
>	if F[n] == -1:
>		a = memFib(n-1, F)
>		b = memFib(n-2, F)
>		F[n] = a + b
>	return F[n]
>```
>in questo modo l’algoritmo effettuerà **esattamente $n$ chiamate ricorsive**, e tenendo conto cche ogni chiamata ricorsiva costa $O(1)$, il tempo di calcolo di `Fib(n)` è $\Theta(n)$: un miglioramento esponenziale rispetto alla versione da cui eravamo partiti !!
>
inoltre: 
>- è possibile eliminare la ricorsione per ottenere la versione iterativa, in cui la complessità asintotica rimane $O(n)$ ma c’è un risparmio di tempo e spazio che era necessario per gestire la ricorsione:
>```python
>def Fib2(n):
>	F = [-1]*(n+1)
>	F[0] = F[1] = 1
>	for i in range(2, n+1):
>		F[i] = F[i-2] + F[i-1]
>	return F[n]
>```
>- è possibile ridurre la complessità di spazio, inzialmente di $\Theta(n)$, tenendo conto che dell’intera tabella basta conservare in memoria, volta per volta, solo gli ultimi due valori calcolati
>
>>[!tip] si nota come la versione ricorsiva dell’algoritmo usa un approccio **top-down**, mentre la versione iterativa usa un approccio **bottom-up** !

## problema 1
studiamo ora un altro problema famoso, risolvibile con programmazione dinamica: 
>[!info] problema 
abbiamo un disco di capacità $C$, e $n$ file di varie dimensione, ciascuna inferiore a $C$. bisogna trovare il sottoinsieme di file che può essere memorizzato sul disco che **massimizza lo spazio occupato**.
progettare un algoritmo che, dati $C$ e la lista $A$, dove $A[i]$ è la dimensione del file $i$, risolva il problema
>>[!warning] osservazione
>>la traccia di questo problema è molto simile ad un altro problema affrontato durante lo studio della tecnica greedy, dove però l’obiettivo era quello di trovare l’insieme **di cardinalità maggiore**, non l’insieme che massimizzasse lo spazio occupato nel disco

per questo tipo di problema, non si conoscono algoritmi efficienti ! i migliori algoritmi si basano su qualche tipo di ricerca esaustiva della soluzione. 
- ciònonostante, è altrettanto facile trovare algoritmi di approssimazione efficienti, con rapporti d’approssimazione costante

### soluzione con divide et impera
per semplicità di espozione di limitiamo a calcolare il valore della soluzione ottima, iniziando da un algoritmo basato sul *divide_et_impera*:
sia $(A,C)$ l’istanza che vogliamo risolvere:
- se la lista dei file risulta vuota o $C== 0$, la soluzione ottima vale $0$
- in caso contrario, l’ultimo file della lista può appartenere o meno alla soluzione ottima:
	- se l’ultimo file non appartiene alla soluzione, i rimanenti $n-1$ file devono essere una soluzione ottima per $(A[n-1], C)$
	- se l’ultimo file appartiene alla soluzione, i rimanenti $n-1$ file devono essere una soluzione ottima per $(A[n-1], C- A[n-1])$
possiamo quindi ricondurci al calcolo della soluzione ottima di due sottoproblemi di dimensione inferiore, in quanto manca all’appello l’ultimo vile, e una volta risolta questi due problemi, ottenuti i valori $v_{1}$ e $v_{2}$ delle loro soluzioni, la soluzione al problema di partenza sarà data da $max(v_{1}, v_{2} + A[-1])$

>[!info] implementazione
>**IDEA**:
>usiamo un indice $i$ per evitare il costo del passaggio della copia di una lista tra le chiamate di funzione
>```python
># i è inizializzato a len(A)
>def es(A, i, C):
>	if i == 0 or C == 0:
>		return 0
>	lascio = es(A, i-1, C) # lista senza l'elemento finale
>	if A[i-1] > C:
>		return lascio
>	prendo = A[i-1] + es(A[:i-1], C - A[i-1])
>	return max(lascio, prendo)
>```
l’implementazione dell’algoritmo costa:
>$$
>T(n,C) = T\Big(n-1, C\Big) + T\Big(n-1, C-A[n-1]\Big) + \Theta(1)
>$$
>che risolta dà $T(n, n-1) = \Omega(2^{\frac{n}{2}})$

>[!info] spiegazione complessità
![[Pasted image 20250504120641.png]]
per evitare di fare questo lavoro inutile, ricorriamo alla memoizzazione: utilizziamo una tabella in cui salviamo i risultati via via calcolati per poterli riutilizzare se necessario
>la tabella $T$ avrà dimensione $n\times (C+1)$, e nella cella $T[i][j]$ memorizzeremo il valore ottenuto dalla soluzione del sottoproblema in cui si hanno solo i primi $i$ file e un disco di capacità $j$

>[!info] implementazione divide et impera memoizzato
>```python
>def es(A, C):
>	T = [ [-1]*(C+1) for i in range(len(A) + 1)]
>	return mem_es(A, len(A), C, T)
>
>def mem_es(A, i, c, T):
>	if T[i][c] == -1:
>		if i == 0 or c == 0:
>			T[i][c] == 0
>		else: 
>			lascio = mem_es(A, i-1, c, T)
>			T[i][c] = lascio
>			if A[i-1] <= C: 
>				prendo = mem_es(A, i-1, c - A[i-1], T)
>			T[i][c] = max(lascio, prendo)
>	return T[i][c]
>```
>il tempo di calcolo della versione memoizzata è limitato dalla dimensione della tabella, in quanto la funzione `mem_es()` esegue $O(1)$ operazioni in ogni chiamata, e il numero totale di chiamate non può superare la dimensione della tabella
>- la complessità dell’algoritmo è dunque $O(nC)$ (in particolare è $\Theta(nC)$)

### soluzione con programmazione dinamica
