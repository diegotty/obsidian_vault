---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-29T14:35
completed: false
---
## link state
in **link state**, lo stato di un link indica il costo associato ad link (collegamento)
ogni nodo deve conoscere i costi di tutti i collegamenti della rete
### link state database
il **link state database** è una matrice che contiene la mappa completa della rete
>[!info] come si costruisce il LSDB
>innanizitutto ogni nodo della rete deve conoscere i propri vicini e i costi dei collegamenti verso di loro. se ciò si verifica:
>- ogni nodo invia un messaggio di `hello` a tutti i suoi vicini
>- ogni nodo riceve gli `hello` dei vicini e crea la lista dei vicini con i relativi costi dei collegamenti

tutti i nodi applicano dijkstra quindi link state è distribi