---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-27T15:35
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

un grafo di una rete di calcolatori, il **cammino a costo minimo tra 2 nodi** viene calcolato da un **algortimo d’istradamento**
## algoritmi d’istradamento con distance vector
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
i percorsi $(a\to y), (b\to y), (c\to y)$ sono percorsi a costo minimo precedentemente stabiliti
>$$
D_{xy} = min\{(c_{xa} + D_{ay}), (c_{xb} + D_{by}), (c_{xc} + D_{cy})\}
>$$

### vettore distanza
il **vettore distanza** è un array monodimensionale che rappresenta un **albero a costo minimo**: una combinazione di percorsi a costo minimo dalla radice dell’albero verso tutte le destinazioni
- essendo monodimensionale, non fornisce il percorso da seguire per giungere alla destinazione, ma **solo i costi minimi** per le destinazioni
>[!example] esempio
![[Pasted image 20250427142315.png]]
>il vettore distanza del nodo $A$ !

ogni nodo della rete, quando viene inizializzato, crea un vettore distanza **iniziale**, con le informazioni che il nodo riesce ad ottenere dai propri vicini (cioè i nodi a cui è direttamente collegato)
- per creare il vettore dei vicini, invia messaggi di `hello` attraverso le sue interfacce (e lo stesso fanno i vicini) e scopre le identità dei vicini e la sua distanza da ognuno di essi
- quindi il vettore distanza iniziale rappresenta il valore dei percorsi minimi verso i vicini
- dopo che ogni nodo ha creato il suo vettore distanza iniziale, ne invia una copia ai suoi vicini, che aggiornano il proprio vettore applicando l’equazione di bellman-ford !
>[!example]
![[Pasted image 20250427142912.png]]
**nota**: con $X[]$ si indica “l’intero vettore $X$”
>- dati i vettori distanza inziali, osserivamo cosa succede quando $B$ riceve una copia del vettore distanza di $A$
![[Pasted image 20250427143221.png]]
>- ora osserviamo cosa succede quando $B$ riceve una copia del vettore $E$
![[Pasted image 20250427143315.png]]

>[!info] logica dell’algoritmo d’istradamento con vettore distanza
l’algoritmo quindi usa la seguente idea di base:
>- ogni nodo invia una copia del proprio vettore distanza a ciascuno dei suoi vicini
>- quando un nodo $x$ riceve un nuovo vettore distanza $DV$, da qualcuno dei suoi vicini, lo salva e usa la formula di bellman-ford per aggiornare il proprio vettore distanza
>- se il vettore distanza del nodo $x$ è cambiato per via di tale passo di aggiornamento, il nodo $x$ manderà il proprio vettore distanza aggiornato a ciascuno dei suoi vicini
>>[!figure]- illustrazione
>![[Pasted image 20250427143748.png]]

>[!info] implementazione dell’algoritmo distance vector
```
```
### problema nella modifica dei costi
con le modalità di aggiornamento definite nell’algoritmo, si può verificare il **problema del conteggio all’infinito**: le buone notizie viaggiano in fretta, e le notizie cattive si propagano lentamente
>[!example] esempio di problema del conteggio all’infinito
![[Pasted image 20250427144841.png]]
>in questo esempio il costo di ogni collegamento è unitario
>16 == $\infty$
>dato che bellman-ford prende il minimo, per aggiornare correttamente i due router servono diversi aggiornamenti !

esistono 2 soluzioni al problema del conteggio all’infinito:
- **split horizon**: invece di inviare la tabella attraverso ogni interfaccia, ciascun nodo invia solo una parte della sua tabella tramite le interfacce: se il nodo $B$ ritiene che il percorso ottimale per raggiungere il nodo $X$ passi attraverso $A$, allora **NON** deve fornire questa informazione ad $A$ (l’informazione è arrivata **da** $A$, e quindi la conosce già)
	- nell’esempio di sopra, $B$ elimina la riga di $X$ dalla tabella prima di inviarla ad $A$
- **poisoned reverse** (inversione avvelenata): si pone a $\infty$ il valore del costo del percorso che passa attraverso il vicino a cui si sta inviando il vettore
	- nell’esempio di sopra, $B$ pone a $infty$ il costo verso $X$ quando invia il vettore ad $A$
# RIP
il **RIP** (**routing information protocol**) è un protocollo a vettore distanza, 
- è tipicamente incluso in UNIX BSD dal 1982
- la metrica di costo del RIP è la **distanza misurata in hop**, in cui 15 è il numero massimo di hop, e il valore 16 indica $\infty$
	- ogni link ha costo unitario
>[!example] esempio
![[Pasted image 20250427153006.png]]
>- è necessario un hop per passare da un router a un host
>- è necessario un hop per passare da un router ad un altro router 
// how ????? per passare ad un altro router attraversa 2 collegamenti ?
## route determination algorithm
periodicamente, ogni router su cui è arrivo RIP, manda la propria tabella di routing (vettore distanza) per fornire informazioni agli altri router riguardo i network e gli host a cui il router sa arrivare. qualunque router nella stessa network del router che sta mandando informazioni potrà aggiornare la propria tabella in base alle informazioni che ricevono. 
- ogni router che riceve un messaggio da un altro router, nella stessa network, che dice di poter raggiungere