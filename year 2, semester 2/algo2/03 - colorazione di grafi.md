---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-08T09:51
completed: false
---
# colorazione di grafi
>[!info] il problema: 
>dato un grafo connesso **e indiretto** $G$ ed un intero $k$, vogliamo sapere se è possible colorare i nodi del grafo in modo che i nodi adiacenti abbiano sempre colori distinti. in quel caso, il grafo si dice $k \text{-colorabile}$

>[!example] esempio di grafo 3-colorabile
![[Pasted image 20250307225409.png]]
 
>[!info]- lore: teorema dei 4 colori
il teorema dei 4 colori dice che un [[01 - grafi#grafi planari|grafo planare]] richiede al più 4 colori per essere colorato
>- il problema venne posto per la prima vola nel 1852, e la prima dimostrazione fu proposta solo nel 1879
>- solo nel 1890 si scoprì che la dimostrazione contenteva un sottile errore, con cui però si riuscì a provare che 5 colori sono **sempre** sufficienti per colorare una mappa
>- una dimostrazione effettiva del teorema fu trovata solo nel 1977, e si basava sulla riduzione del numero infinito di grafi planari a 1936 possibili configurazioni, per le quali la validità del teorema viene verificata caso per caso, con l’ausilio di un calcolatore
>- l’utilizzo di un algoritmo per una dimostrazione, però, fece imbestialire i matematici che ne contestarono la validità, sia per l’impraticalità di una verifica manuale di tutti i casi possibili, sia per l’impossibilità di avere la certezza che l’algoritmo sia implementato correttamente
>- nel 2000 è stata trovata una dimostrazione che richiede l’utilizzo della teoria dei gruppi. 

>[!warning] in generale, un grafo può richiedere anche $\Theta(n)$ colori

non è noto nessun algoritmo polinomiale che, dato un grafo planare $G$, determini se $G$ è 3-colorabile. get to work people !!!! 
## algoritmo per 2-colorabilità
non è però difficile progettare un algoritmo che determini se un grafo è 2-colorabile:
$$
\text{un grafo è 2-colorabile} \iff \text{non contiene cicli di lunghezza dispari}
$$
in quanto un grafo di lunghezza dispari rende impossibile la colorazione del grafo con solo 2 colori
>[!info] algoritmo per 2-colorare un grafo
usiamo un algoritmo per 2-colorare il grafo, che per ipotesi non ha cicli dispari:
>1. colora il nodo 0 con il colore 0
>2. effettua una visita DFS a partire dal nodo 0. nel corso della visita, assegna a ciascun nodo $x$ un colore tra 0 e 1, in modo che sia diverso dal colore assegnato al nodo padre di $x$

>[!dimostrazione] prova di correttezza dell’algoritmo di sopra
siano $x$ e $y$ due nodi adiacenti in $G$. consideriamo i due possibili casi e facciamo vedere che in entrambi i casi i due nodi al termine dell’algoritmo avranno colori opposti:
- l’arco $(x,y)$ viene attraversato durante la visita: in questo caso banalmente i due nodi hanno colori distinti
- l’arco $(x,y)$ non viene attraversato durante la visita: 
