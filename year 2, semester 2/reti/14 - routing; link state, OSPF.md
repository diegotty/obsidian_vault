---
related to: "[[13 - routing; distance vector, RIP]]"
created: 2025-03-02T17:41
updated: 2025-05-04T14:03
completed: false
---
dopo aver visto il routing usante [[13 - routing; distance vector, RIP#algoritmi d’istradamento con distance vector|distance vector]], studiamo gli algoritmi di routing usanti **link state**
## link state
lo stato di un link indica il costo associato al link (collegamento). ogni nodo deve conoscere i costi di tutti i collegamenti della rete, e la mappa completa di tutti i link state deve essere mantenuta dal **link state database** (**LSDB**)
- se il costo è $\infty$ significa che il collegamento non esiste oppure è stato interrotto
>[!example] esempio di LSDB
![[Pasted image 20250504140304.png]]
il **LSDB** viene rappresentato come una matrice, è unico per tutta la rete, e ogni nodo ne possiede una copia

