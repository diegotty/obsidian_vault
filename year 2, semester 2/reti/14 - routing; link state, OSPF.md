---
related to: "[[13 - routing; distance vector, RIP]]"
created: 2025-03-02T17:41
updated: 2025-05-04T14:08
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

tutti i nodi applicano dijkstra quindi link state è distribi