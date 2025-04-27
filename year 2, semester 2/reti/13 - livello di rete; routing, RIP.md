---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-27T14:18
completed: false
---
# routing
il routing si occupa di trovare il miglior percorso da far percorrere ad un pacchetto e inserilo nella tabella di routing (= tabella di forwarding)
- quindi il routing costruisce le tabelle, che vengono poi uste dal [[12 - livello di rete; DHCP, NAT, forwarding, ICMP#forwarding dei datagrammi IP|forwarding]]
>[!info] grafo di una rete di calcolatori
![[Pasted image 20250427140019.png]]
$\text{grafo:} G=(N,E)$
$N=\text{insieme di nodi (router) = \{u,v,w,x,y,z\}}$
$E=\text{insieme di archi (collegamenti)= \{(u,v), (u,x), (v,x), (v,w), (x,w), (x,y),(w,y),(w,z), (y,z)\}}$
>- un path nel grafo $G$ è una sequenza di nodi $(x_{1},x_{2},\dots,x_n)$ tale che ognuna delle coppie $(x_{1},x_{2}), (x_{2},x_{3}),\dots, (x_{n-1},x_{n})$
>- $c(x,x')$ è il costo del collegamento $(x,x')$, ed il costo di un cammino è la somma di tutti i costi degli archi lungo il cammino
>
>il costo di un cammino può rappresentare:
>- la lunghezza fisica del collegamento
>- la velocità del collegamento
>- il costo monetario associato al collegamento

## algoritmi d’istradamento
un grafo di una rete di calcolatori, il **cammino a costo minimo tra 2 nodi** viene calcolato da un **algortimo d’istradamento**
### distance vector 
l’algoritmo di istradamento con **vettore distanza** (distance vector - DV) è:
- **distribuito**: ogni nodo riceve informazione dai vicini e opera su quelle
- **asincrono**: non richiede che tutti i nodi operino al passo con gli altri
e si basa su :
### equazione di bellman-ford
si basa sulla **formula di bellman-ford**, che definisce: 
$$
D_{x}(y) := \text{il costo del percorso a costo minimo dal nodo $x$ al nodo $y$)}
$$
che si calcola: 
$$
D_{x}(y)=min_{v}\{c(x,y) + D_{v}(y)\}
$$
dove $min_{v}$ riguarda tutti i vicini di $x$
>[!example] rappresentazione grafica
![[Pasted image 20250427141721.png]]
i percorsi $(a\to y), (b\to y), (c\to y)$ sono percorsi a costo minimo
>$$
D_{xy} = min\{(c_{xa} + D_{ay}), (c_{xb} + D_{by}), (c_{xc} + D_{cy})\}
>$$
# RIP
il **RIP** (**routing information protocol**)


16 == inf