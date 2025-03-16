---
related to: "[[01 - grafi]]"
created: 2025-03-02T17:41
updated: 2025-03-16T19:34
completed: false
---
>[!example] problema
>abbiamo tre contenitori di capienza 4, 7 e 10 libri. inizialmente i contenitori da 4 e 7 litri sono pieno d’acqua e quello da 10 è vuoto.
>possiamo effettuare un solo tipo di operazione: versare acqua da un contenitore ad un altro fermandoci quando il contenitore sorgente è vuoto o uello destinazione è pieno.
**esiste una sequenza di operazioni di versamento che termina lasciando esattamente due litri d’acqua nel contenitore da 4 o nel contenitore da 7 ?**

possiamo modellare il problema con un grafo $G$:
- i nodi di $G$ sono i possibili stati di riempimento dei 3 contenitori: ogni nodo rappresenta una configurazione $(a,b,c)$ dove $a$ è il numero di litri nel contenitore di 4, $b$ rappresenta il contenitore di 7 litri, etc.
- metto un arco dal nodo $(a,b,c)$ al nodo $(a',b', c')$ se dallo stato $(a,b,c)$ è possibile passare allo stato $(a',b',c')$ con un versamento lecito
>[!example] frammento del grafo $G$ !:
![[Pasted image 20250316181633.png]]

per risolvere il problema, basterà chiedersi se nel grafo diretto $G$, almeno uno dei nodi $(2,?,?)$ o $(?,2,?)$ è raggiungibile a partire dal nodo $(4,7,0)$
- possiamo quindi aggiungere al grafo un ulteriore **nodo pozzo**(nodo con solo archi entranti) $(-1,-1,-1)$, con archi entranti che provengono da dai nodi $(2,?,?)$ o $(?,2,?)$, e chiederci se il nodo pozzo è raggiungibile da $(4,7,0)$ (se è raggiungibile da $(4,7,0)$, vuol dire il cammino che è passato da un nodo che ha la configurazione corretta (in quanto gli unici archi entranti provengono da tali nodi !!) )
il grafo è composto da 441 nodi ($5 \cdot8 \cdot 11 = 440$ più il nodo pozzo), e comprende anche i nodi che non possono essere raggiunti attraverso versamenti leciti

ora, per risolvere il problema, basta effettuare una visita DFS o BFS (in quanto ogni arco rappresenta un versamento lecito). è quindi risolvibile in $O(n+m)$ !
- se siamo interessati a raggiungere una delle configurazioni target con meno versamenti possibili, possiamo usare la visita BFS, calcolando le distanze minime dal nodo $(4,7,0)$
>[!example] variante del problema
>ci interessa trovare una sequenza(valida come soluzione del problema di sopra) in cui il totale dei litri versati in tutti i versamenti della sequenza è **minimo** rispetto a tutte le sequenze lecite
# grafi pesati
(quando parliamo di grafi pesati, sottointendiamo che il peso è sugli archi, e non sui nodi)
possiamo modellare il nuovo problema come segue: assegniamo un **costo** ad ogni arco, che rappresenta il numero di litri versati nella mossa corrispondente all’arco
>[!example] frazione del nuovo grafo
![[Pasted image 20250316192343.png]]

abbiamo quindi costruito un **grafo pesato**, in quanto ogni arco(possono essere anche i nodi ad avere un peso) ha un costo

anche se il problema ha una struttura diversa, è una generalizzazione del problema dei cammini di lunghezza minima, perchè gli archi, invece di avere tutti lo stesso valore (cioè 1), hanno valori differenti.
notiamo però che possiamo sostituire un arco da $x$ a $y$ di costo $C$ con un cammino di $C-1$ nuovi nodi ! (in questo modo, non dobbiamo gestire il costo, ma lo riusciamo a rappresentare nella lunghezza del cammino tra due configurazioni di contenitori)
- quindi ogni arco diventa un cammino con diversi **nodi dummy**
>[!example]- porzione di grafo con sostituzione
![[Pasted image 20250316192959.png]]

in questo modo, per risolvere il problema basta eseguire una visita BFS nel nuovo grafo, a partire dal nodo $(4,7,0)$ che si ferma non appena trova il nodo $(-1,-1,-1)$
l’algoritmo avrà complessità $O(n'+m')$, dove $n'$ ed $m'$ sono rispettivamente i nodi e gli archi del nuovo grafo
- nel nostro caso, il peso degli archi non può superare 7

so che i nodi raggiungibili devono avere per forza somma totale di 11

- in alcuni casi, può essere un grafo indiretto perche posso sempre tornare indietro , ma non è sempre cosi