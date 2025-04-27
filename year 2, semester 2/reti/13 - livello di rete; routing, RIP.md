---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-27T14:05
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
un path nel grafo $G$ è una sequenza di nodi $(x_{1},x_{2},\dots,x_n)$ tale che ognuna delle coppie $(x_{1},x_{2}), (x_{2},x_{3}),\dots, (x_{n-1},x_{n})$
### equazione di bellman-ford
$$
D_{x}(y)=min_{v}\{c(x,y) + D_{v}(y)\}
$$
dove $min_{v}$ riguarda tutti i vicini di $x$

# RIP
il **RIP** (**routing information protocol**)


16 == inf