---
created: 2025-05-06T13:13
updated: 2025-05-07T23:17
---

sappiamo che gli algoritmi basati sulla tecnica divide_et_impera seguono 3 passi:
1. dividi il problema in sottoproblemi di taglia inferiore
2. risolvi (ricorsivamente) i sottoproblemi di taglia inferiore
3. combina le soluzioni dei sottoproblemi in una soluzione del problema originale
>[!tip]
negli esempi visti finora, i sottoproblemi che si ottenevano dall’applicazione del passo 1 erano tutti **diversi**, pertanto ciascuno di essi veniva individualmente risolto dalla relativa chiamata ricorsiva del passo 2. in molte situazioni però, i sottoproblemi ottenuti al passo 1 possono risultare **uguali**. in tal caso, l’algoritmo basasto sulla tecnica *divide_et_impera* **risolve più volte lo stesso problema, svolgendo lavoro inutile**

>[!warning] differenza tra divide et impera e programmazione dinamica
anche se entrambe le tecniche si basano sul scomporre il problema in sottoproblemi più semplici, differiscono in **come** affrontano e risolvono i sottoproblemi
**divide et impera**: nell’approccio divide et impera, il problema è diviso in sottoproblemi **indipendenti**, che vengono risolti ricorsivamente, per poi combinare le soluzioni. dato che i problemi son diversi tra loro, non ha senso memorizzarli
**programmazione dinamica**: i sottoproblemi in cui viene diviso il problema principale **si ripetono** (il risultato dei sottoproblemi calcolati è necessario più volte): grazie alla **memoizzazione**, vengono risolti solo una volta.
>- usa qundi memoria extra per evitare calcoli ripetuti

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

in termini dell’albero delle chiamate ricorsive, l’algoritmo iterativo calcola i risultati a partire dalle foglie, cioè gli elementi $T[0,c]$, poi i risultati del livello superiore e così via, risalendo l’albero di livello in livello fino alla radice $T[n,C]$
- approccio bottom-up !

>[!info]- algoritmi pseudopolinomiali (non finito)
viene detto **pseudopolinomiale** un algoritmo che risolve un problema in tempo polinomiale quando i numeri presenti nell’input sono codificati in unario
>- l’algoritmo che abbiamo ottenuto sopra, di complessità $O(nC)$ è un esempio di algoritmo pseudopolinomiale ! in quanto ricordiamo che la capacità $C$ del disco, data in input, è codificata con $\log C$ bit, quindi per ottenere un algoritmo polinomiale …. um ngl non ho capito queste cose mi turbano
## ricavare la soluzione dal valore della soluzione
fino ad ora abbiamo usato la tabella usata per risolvere i problemi con la programmazione dinamica solamente per **calcolare il valore** della soluzione ottima. può essere usata anche per **ricavare** (in questo caso, l’insieme di elementi) la soluzione ottima
- quindi in un primo momento ci si può concentrare sul calcolo del valore della soluzione, e successivamente usare la tabella riempita per ritrovare la soluzione a cui quel valore corrisponde, ripercorrendo a ritroso le decisioni prese a partire dal valore ottimo !! (grazie memoizzazione)
>[!example] esempio
![[Pasted image 20250507223317.png]]
le frecce rosse evidenziano le decisioni a partire dall’elemento $T[n, C]$ (elemento che fornisce la soluzione ottima). in particolare:
>- se $T[k,c] = T[k-1, c]$, il $k$-esimo file non è scelto, altrimenti $T[k,c] > T[k-1, c]$ e ciò implica che il $k$-esimo file è stato scelto
![[Pasted image 20250507223340.png]]

>[!info] implementazione
>```python
>def esIterativo(A, C):
>	n = len(A)
>	T = [ [0] * (C+1) for i in range(n+1)]
>	for i in range(1, n+1):
>		for c in range(C+1):
>			if c < A[i-1]:
>				T[i][c] = T[i-1][c]
>			else:
>				T[i][c] = max(T[i-1][c], A[i-1] + T[i-1][c - A[i-1]])
>	# trovo soluzione (dopo aver trovato valore della soluzione)
>	valore = T[n][c]
>	sol = []
>	i = n
>	while i > 0:
>		if T[i][valore] != T[i-1][valore]:
>			sol.append(i-1)
>			valore -= A[i-1]
>		i -= 1
>	return T[n][C], sol
>```
nella prima parte viene pagato $O(nC)$ per il calcolo della tabella, mentre nella seconda parte la tabella viene visitata una riga alla volta, ed il costo è $O(n)$
quind il costo complessivo è $O(nC)$

sinceramente il fatto che si possa fare in modo iterativo è pazzesco ngl
## esercizi proposti in classe
>[!example] problema
vogliamo contare il numero di stringhe binarie di lunghezza $n$ senza 2 zeri consecutivi
>>[!info] soluzione
>per questo tipo di esercizi, tendenzialmente è necessario calcolarsi a mano i primi valori, per capire il pattern per la costruzione dei successivi:
>>- $n=0: \text{res}=1$
>>- $n=1: \text{res}=2$
>>- $n=2: \text{res}=3$
>>usiamo un vettore $T$, con $$T[i] = \text{il numero di stringhe binarie lunghe i senza 2 zeri consecutivi}$$
>aggiungendo una cifra, abbiamo due casi: che sia 0 che sia 1:
>>- se è 1, andranno bene tutti i casi precedenti ($T[n-1]$)
>>- se è 0, andranno bene solo i casi in cui la la penultima cifra **non** è uno zero: sono $T[n-2]$, cioè i casi di con $n-1$ che finiscono con 1 (che sono tutti i casi considerati in $T[n-2]$.
>>
>>dunque
>>$$
>>T[n] = T[n-1] + T[n-2]
>>$$
>>è possibile applicare la formula a partire da $T[i]$

>[!example] problema
vogliamo contare il numero di stringhe binarie di lunghezza $n$ senza 3 zeri consecutivi
>>[!info] soluzione
>usiamo la stessa configurazione dell’esercizio precedente, e ragioniamo nella situazione in cui aggiungiamo una cifra:
>>- se è 1, andranno bene tutti i casi precedenti $(T[n-1])$
>>- se è 0, andranno bene i casi in cui le ultime 2 cifre non sono entrambe 0: le cifre che hanno come penultima cifra il valore 1 ($T[n-2]$), e le cifra che hanno come terz’ultima cifra il valore 1 ($T[n-3]$)
>dunque
>>$$
>T[n] = T[n-1] + T[n-2] + T[n-3]
>>$$

>[!example] problema
abbiamo $n$ (con $n\geq1$) persone da distribuire in un albergo con stanze singole o doppie. in quanti modi si possono distribuire le persone ?
>>[!info] soluzione
>>- $n=1: \text{res}=1$
>>- $n=2: \text{res}=2$
>>- $n=3: \text{res}=4$
>usiamo la stessa struttura dell’esercizio sopra
>aggiungendo una persona:
>>- la metto in una camera singola: le possibili distribuzioni sono le stesse di $T[i-1]$
>>- la metto in una camera doppia: devo accoppiarlo con una persona singola: pensiamo che sia penultima persona: le distribuzioni in cui la penultima persona è in un letto singolo sono $T[i-2]$. ciò vale per ogni persona tra le $i-1$ !!
>>$$
>T[i] = T[i-1] + T[i-2]\cdot (i-1)
>>$$
>>
>>**implementazione**: 
>>```python
>>def distribuzioni(n):
>>	i = 1
>>	T = [0]*(n+1)
>>	T[1], T[2] = 1, 2
>>	while(i < n+1):
>>		T[i] = T[i-1] + T[i-2]*(i-1)
>>```
