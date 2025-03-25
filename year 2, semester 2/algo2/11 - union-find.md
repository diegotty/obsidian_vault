---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-25T09:12
completed: false
---
# union-find
**union-find**, noto anche come **disjoint-set union**(**DSU**), è una struttura dati per gestire insiemi **disgiunti**, in particolare per facilitare operazioni di **unione** e **ricerca** efficienti su tali insiemi
le tre operazioni fondamentali di union-find sono:
- `Crea(S)`: restituisce una struttura dati union-find sull’insieme $S$ di elementi, **dove ciascun elemento è in un insieme separato**
- `Find(x, C)`: restituisce il nome dell’insieme (della struttura dati $C$) a cui appartiene l’elemento $x$
- `Union(A, B, C)`: modifica la struttura dati $C$ fondendo la componente $A$ con la componente $B$, e restituisce **il nome della nuova componente**
>[!info]- quando è utile la struttura dati union-find
>dato che ci permette una gestione efficiente di insiemi disgiunti, union-find è utile in diversi contesti, tra cui l’evoluzione di un grafo nel tempo attraverso l’aggiunta di archi. in questo caso, gli insiemi disgiunti rappresentano le componenti connesse del grafo. guarda [[10 - spanning tree#algoritmo di Kruskal| l’algoritmo di kruskal !]]

>[!info] concetto di nome dell’insieme
il nome dell’insieme è il modo di rappresentare un insieme (nel nostro caso, rappresentiamo la componente connessa, con il nome di uno dei nodi al suo interno)
>
>la scelta fatta nelle implementazioni che seguono è quella di scegliere come nome dell’insieme quello di un particolare elemento dell’insieme stesso. in particolare, pensiamo di assegnare all’insieme **il nome dell’elemento massimo in esso contenuto**
>- discorso molto simile a classe di equivalenza e scelta dei rappresentati, ma in questo caso ci serve un solo rappresentante per identificare la componente
## implementazione
per implementare la struttura dati per $n$ elementi  nel modo più semplice, mantienamo il **vettore** $C$ delle $n$ componenti ([[03 - colorazione di grafi, componenti connesse#componente connessa|vettore delle componenti]])
- in questo modo, quando la componente $i$ verrà fusa con la componente $j$, se $i > j$ allora tutte le occorrenze di $j$ nel vettore $C$ verranno sostituite da $i$
>[!warning] esistono diverse implementazioni della struttura union-find, e in ognuna variano i costi per le operazioni `find()` e `union()`! 
>noi ne studieremo 2:
>- `union()` in $O(n)$ e `find()` in $O(1)$
>- `union()` in $O(1)$ e `find()` in $O(\log n)$
>
> esistono poi implementazioni con euristica e altre cose molto cool

>[!info] `union()` in $\Theta(n)$, `find()` in $\Theta(1)$

```python
def Crea(G):
	C = [i for i in range(len(G))]
	return C

def Find(u, C):
	return C[u]

def Union(a, b, C)
```

se ho due alberi da unire, devo rendere figlio l’albero con meno nodi
- in questo modo la profondità dell’albero creato dalle fusioni non avrà mai altezza maggiore di logn
prova x induzione
- se un albero ha altezza k, avrà al suo interno almeno 2^k nodi (dimostreremo)

- quindi se h=log, al suo interno ci possono essere almeno n nodi

esercizio ::::
- dato che è un dag, so che il costo da u a u è 0.
- dato che è un dag, tutti i nodi prima di u (nella rappresentazione di sort topologico), sono irraggiungibili da u

quando arrivo ad un nodo, so che ho calcolato le distanze di ogni nodo entrante in tale nodo.