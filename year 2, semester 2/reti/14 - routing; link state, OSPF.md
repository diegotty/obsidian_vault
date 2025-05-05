---
related to: "[[13 - routing; distance vector, RIP]]"
created: 2025-03-02T17:41
updated: 2025-05-05T18:41
completed: false
---
dopo aver visto il routing usante [[13 - routing; distance vector, RIP#algoritmi d’istradamento con distance vector|distance vector]], studiamo gli algoritmi di routing usanti **link state**
## link state
lo stato di un link indica il costo associato al link (collegamento). ogni nodo deve conoscere i costi di tutti i collegamenti della rete, e la mappa completa di tutti i link state deve essere mantenuta dal **link state database** (**LSDB**)
- se il costo è $\infty$ significa che il collegamento non esiste oppure è stato interrotto
### link state database
il **link state database** è una matrice che contiene la mappa completa della rete. è unico per tutta la rete, ed ogni nodo ne possiede una copia
>[!example] esempio di LSDB
![[Pasted image 20250504140304.png]]

>[!info] come si costruisce il LSDB
>innanizitutto ogni nodo della rete deve conoscere i propri vicini e i costi dei collegamenti verso di loro. se ciò si verifica:
>- ogni nodo invia un messaggio di `hello` a tutti i suoi vicini
>- ogni nodo riceve gli `hello` dei vicini e crea la lista dei vicini con i relativi costi dei collegamenti
> 	- la lista (vicino, costo) viene chiamata **LS packet** (**LSP**)
>- ogni nodo esegue un **flooding** dei LSP: 
>	 - invia a tutti i vicini il proprio LSP
>	 - quando riceve l’LSP di un vicino, se è un nuovo LSP allora lo inoltra a tutti i suoi vicini diretti eccetto quello da cui lo ha ricevuto
>
>>[!example] esempio
>![[Pasted image 20250504141221.png]]

## algoritmo d’instradamento a stato del collegamento
l’algoritmo di instradamento usato da algoritmi con **link state** è l’**algoritmo di dijkstra** ! ([[09 - algoritmo di dijkstra|precedentemente studiato ad algo2]])
sappiamo che la topolgia di rete e tutti i costi dei collegamenti sono noti a tutti i nodi: con l’algoritmo di dijkstra ogni nodo calcola il cammino a costo minimo verso tutti gli altri nodi della rete !
- tutti i nodi applicano dijkstra, in modo indipendente, quindi link state è un routing distribuito
una volta applicato dijkstra, ogni nodo crea una **tabella d’inoltro** !
### algoritmo di dijkstra
definiamo la seguente notazione:
- $N$: insieme dei nodi rella rete
- $c(x,y)$: costo del collegamento dal nodo $x$ al nodo $y$
- $D(v)$: costo del cammino minimo dal nodo origine alla destinazione $v$, per quanto riguarda l’iterazione corrente (il nodo radice che sta applicando dijkstra)
- $p(v)$: immediato predecesore di $v$ lungo il cammino
- $N’$: sottoinsieme di nodi per cui il cammino a costo minimo dall’origine è definitivamente noto
>[!info] implementazione
![[Pasted image 20250505182729.png]]

## confronto tra algoritmi LS e DV
- **complessità dei messaggi**:
	- **LS**: con $n$ nodi, $E$ collegamenti, implica l’inviio di $O(nE)$ messaggi !
	- **DV**: richiede scambi tra nodi adiacenti, e il tempo di convergenza può variare
- **robustezza**:
	- **LS**: se un router funziona male, può comunicare via broadcast un costo sbagliato per uno dei suoi collegamenti connessi (ma non per altri !). inoltre i nodi si occupano di calcolare soltanto le proprie tabelle
	- **DV**: un nodo può comunicare **cammini** a costo minimo errati a tutte le destinazioni. inoltre la tabella di ciascun nodo può essere usata dagli altri, quindi un calcolo erraro si può facilmente diffondere per l’intera rete
	quindi **OSPF** è più robusto di **RIP**
- **velocità di convergenza**:
	- **LS**: l’algoritmo ha complessità $O(n^2)$
	- **DV**: