---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-25T08:42
completed: false
---
# union-find
**union-find**, noto anche come **disjoint-set union**(**DSU**), è una struttura dati per gestire insiemi **disgiunti**, in particolare per facilitare operazioni di **unione** e **ricerca** efficienti su tali insiemi
le tre operazioni fondamentali di union-find sono:
- `Crea(S)`: restituisce una struttura dati union-find sull’insieme $S$ di elementi, **dove ciascun elemento è in un insieme separato**
- `Find(x, C)`: restituisce il nome dell’insieme (della struttura dati $C$) a cui appartiene l’elemento $x$
- `Union(A, B, C)`: modifica la struttura dati $C$ fondendo la componente $A$ con la componente $B$, e restituisce **il nome della nuova componente**
>[!info] quando è utile la struttura dati union-find
>

## union-find
la struttura dati **UNION** e **FIND**


se ho due alberi da unire, devo rendere figlio l’albero con meno nodi
- in questo modo la profondità dell’albero creato dalle fusioni non avrà mai altezza maggiore di logn
prova x induzione
- se un albero ha altezza k, avrà al suo interno almeno 2^k nodi (dimostreremo)

- quindi se h=log, al suo interno ci possono essere almeno n nodi

esercizio ::::
- dato che è un dag, so che il costo da u a u è 0.
- dato che è un dag, tutti i nodi prima di u (nella rappresentazione di sort topologico), sono irraggiungibili da u

quando arrivo ad un nodo, so che ho calcolato le distanze di ogni nodo entrante in tale nodo.