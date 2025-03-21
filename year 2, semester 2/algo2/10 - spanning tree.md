---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-21T10:11
completed: false
---
# spanning tree
uno **spanning tree**(albero di copertura) è l’albero in cui esiste una unica componente connessa, con il minimo numero di archi
- il costo dello spanning tree è la somma dei pesi di tutti gli archi 
>[!info] problema: minimo spanning tree
>dato un grafo non diretto, pesato e connesso $G$, vogliamo conoscere lo spanning tree con costo minimo

la soluzione è un albero(grafo connesso aciclico) perchè qualunque ciclo sarebbe una ridondanza (l’eliminazione di un qualunque arco del ciclo non farebbe perdere la connessione e diminuirebbe il costo della soluzione). andiamo quindi alla ricerca in $G$ di un albero che **“copre”** l’intero grafo e la somma dei costi dei suoi archi sia minima !
data l’importanza di di questo problema, esistono diversi algoritmi per risolvere questo problema. noi vedremo il seguente:
## algoritmo di Kruskal
>[!info] logica
>- parti con il grafo $T$ che contiene tutti i nodi di $G$ e nessun arco di $G$.
>- considera uno dopo l’altro gli archi del grafo $G$ in ordine di costo crescente.
>- se l’arco forma ciclo in $T$ con gli archi già presi, allora non prenderlo, altrimenti inseriscilo in $T$.
>- al termine restituisci $T$

l’algoritmo rientra perfettamente nel paradigma nella tecnica greedy !
### correttezza
introduciamo lo pseudocodice dell’algoritmo:
```
kursal(G):
	T = set()
	inizializza E con gli archi di G
	while E != []:
		estrai da E un arco (x,y) di peso minimo
		if l'inserimento di (x,y), in T non crea ciclo con gli archi in T:
			inserisci l'arco (x,y) in T
	return T
```
per dimostrare la correttezza dell’algoritmo, dobbiamo far vedere che al termine dell’algoritmo $T$ è un spanning tree e che non c’è un altro spanning tree che costa meno

>[!dimostrazione] dimostrazione
>**produce uno spanning tree**: 
>supponiamo per assurdo che al termine in $T$ ci sia più di una componente. 
>dato che il grafo di partenza $G$ per ipotesi è connesso, deve esistere (in $G$) un arco che connette le due componenti. dato che non è stato scelto dall’algoritmo, ciò implica che quando è stato estratto, avrebbbe creato un ciclo. (per creare un ciclo, sarebbe dovuto esistere già un cammino che collegava le due componenti). ma dato che non crea ciclo ad algoritmo terminato, non creava ciclo neanche al momento in cui è stato estratto, quindi sarebbe stato aggiunto a $T$ e non scartato
>
**non c’è uno spanning tree per $G$ che costa meno di $T$**:
>tra tutti i minimi spanning tree per $G$, prendiamo quello che differisce nel minor numero di archi da $T$: lo spanning tree $T^*$.
supponiamo per assurdo che il costo dell’albero $T$ prodotto dall’algoritmo abbia costo maggiore (e quindi sia diverso) di, $T^*$. necessariamente ci sarà un arco che compare in $T$ e non in $T^*$ (in quanto i due spanning tree sono diversi). se io aggiungo tale arco in $T^*$, si crea un ciclo. ciò vuol dire che non è possibile che tutti gli alberi partecipanti al ciclo appartengano a $T$ (in quanto $T$ non ha cicli). ne esiste almeno uno che non appartiene. se io toglo tale arco a $T^*$. quindi ora $T^*$ è più simile a $T$. ma in questo modo, ho trovato un altro albero che è ottimo e ha più archi in comune con $T$. inoltre tale nuovo albero non può costare più di $T^*$ in quanto gli archi in $T$ sono scelti per ordine crescente, quindi l’arco che ho copiato da $T$ ha costo minore o uguale a quello tolto da $T^*$, in quanto altrimenti $T$ avrebbe considerato prima l’arco tolto da $T^*$.  quindi il nuovo albero ha sicuramente costo ≤ a $T^*$

`sort()` - flavio sperandeo

sort() su una lista di tuple sorta sul primo valore di ogni tupla

devo rifare per forza la visita (che pero costa $O(n)$, perchè n+m = massimo $n+(n-1)$)

```python
def connessi(x, y, T);
	visitati = [0] * len(T)
	return connessiR(x, y, T)
def connessiR(a, b, T):
	visitati[a] = 1
	for z in T[a]:
		if z == b: return True
		if not visitati[z] and connessiR(z, b, T): return True
	return False	
```