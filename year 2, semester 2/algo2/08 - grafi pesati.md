---
related to: "[[01 - grafi]]"
created: 2025-03-02T17:41
updated: 2025-03-16T18:18
completed: false
---
>[!info] problema
>abbiamo tre contenitori di capienza 4, 7 e 10 libri. inizialmente i contenitori da 4 e 7 litri sono pieno d’acqua e quello da 10 è vuoto.
>possiamo effettuare un solo tipo di operazione: versare acqua da un contenitore ad un altro fermandoci quando il contenitore sorgente è vuoto o uello destinazione è pieno.
**esiste una sequenza di operazioni di versamento che termina lasciando esattamente due litri d’acqua nel contenitore da 4 o nel contenitore da 7 ?**

possiamo modellare il problema con un grafo $G$:
- i nodi di $G$ sono i possibili stati di riempimento dei 3 contenitori: ogni nodo rappresenta una configurazione $(a,b,c)$ dove $a$ è il numero di litri nel contenitore di 4, $b$ rappresenta il contenitore di 7 litri, etc.
- metto un arco dal nodo $(a,b,c)$ al nodo $(a',b', c')$ se dallo stato $(a,b,c)$ è possibile passare allo stato $(a',b',c')$ con un versamento lecito
>[!example] frammento del grafo $G$ !:
![[Pasted image 20250316181633.png]]

per risolvere il problema, basterà chiedersi se nel grafo diretto $G$, almeno uno dei nodi $(2,?,?)$ o $(?,2,?)$

# grafi pesati
(sottointendiamo che il peso è sugli archi, e non sui nodi)


ci sono 5 possibilità per a, 8 possibilità per b, e 11 posibilità per c (anche se alcuni nodi non potranno mai essere raggiunti)
metto un arco quando esiste un versamento che porta dall configurazione a alla configurazione b 
- in alcuni casi, può essere un grafo indiretto perche posso sempre tornare indietro , ma non è sempre cosi

so che i nodi raggiungibili devono avere per forza somma totale di 11

uhhh qualcosa con nodo pozzo

un modo per risolvere il problema: considero le considerazioni con 2L in primo o osecondo bucket.
creo (??) un nodo pozzo e ci mando tutti gli archi dei bucket che ho considerato

se posso arrviare a uno dei bucket considerati, arriverò anche al nodo pozzo


se vogliamo sapere, in caso affermativo, il percorso con il minor numero di litri scambiati tra bucket, devo usare un grafo pesato

posso, per eliminare il grafo pesato, farlo diventare un grafo normale, in cui la lunghezza del cammino tra due nodi rappresenta il peso (aggiungendo dei nodi fittizi) 

quindi ogni arco diventa più archi con nodi dummy