---
created: 2024-12-21
related to: 
updated: 2024-12-21T08:35
---
# deadlock
un deadlock si verifica quando:
- ogni transazione in un insieme $T$ è in attesa di ottenere un lock su un item sul quale qualche altra transazione nell’insieme $T$ mantiene un lock, e quindi:
- ogni transazione rimane bloccata
- nessuna transazione rilascia i lock (essendo bloccata su `lock(X)`, non arriva alla riga di codice `unlock(Y)`(Y item su cui ha il lock))
- ciò può bloccar anche transazioni che non sono in $T$ !

## soluzioni per il deadlock
per verificare la presenza di una situazione di deadlock, si mantiene il **grafo di attesa**:
- nodi: le transazioni
- archi: c’è un arco $T_{1} \to T_{2}$ se la transazione $T_{1}$ è in attesa di ottenere un lock su un item sul quale $T_{2}$ mantiene un lock
se in tale grafo c’è un ciclo, si sta verificando una situazione di stallo che coinvolge **le transazioni nel ciclo**, e va risolta

per risolvere una situazione di deadlock, una transazione nel ciclo viene **rolled-back** e successivamente viene fatta ripartire
quando avviene un **roll-back**:
1. la transazione è abortita
2. i suoi effetti sulla BD vengono annullati, ripristinando i valori dei dati precedenti l’inizio della sua esecuzione
3. tutti i lock mantenuti dalla transazione vengono rilasciati

per evitare il deadlock, si adottano protocolli opportuni
- es: si ordinano gli item e si impone alle transazioni di richiedere i lock necessari seguendo tale ordine (come in [[intro al deadlock#prevenire|SO per evitare l’attesa circolare !]],): in tal modo non ci possono essere cicli nel grafo di attesa (e non si può verificare un deadlock)
>[!info]- dimostrazione
![[Pasted image 20241221082026.png]]
# livelock
un **livelock** si verifica quando una transazione aspetta indefinitivamente che gli venga garantito un lock su un certo item
questo problema può essere risolto:
- con una strategia FCFS (first come, first serve)
- eseguendo le transazioni in base alle loro priorità, e aumentando la priorità di una transazione all’aumentare del tempo in cui rimane in attesa
# abort di una transazione
una transazione può essere abortita perchè:
1. la transazione esegue un’operazione non corretta (divisone per 0, accesso non consentito)
2. lo scheduler rileva un deadlock
3. lo scheduler fa abortire la transazione per garantire la serializzabilità ([[30 - timestamp|timestamp]])
4. si verifica un malfunzionamento hardware o software
## punto di commit
il **punto di commit** di una transazione è il punto in cui la transazione:
- ha ottenuto tutti i lock che gli sono necessari
- ha effettuato tutti i calcoli nell’area di lavoro
e quindi non può più essere abortita a causa dei punti 1-3

i **dati sporchi** sono quindi i dati scritti da una transazione sulla BD prima che abbia raggiunto il punto di commit
## roll-back a cascata
quando una transazione $T$ viene abortita, devono essere annulati gli effetti sulla BD prodotti da $T$, e da qualsiasi transazione che abbia letto dati sporchi

come abbiamo visto:
- l’uso di [[26 - lock binario, lock a 2 fasi#lock binario|lock binario]] permetete di risolvere il problema del ghost update, ma non quello della lettura di un dato sporco, ne quello dell’aggregato
- il protocollo di [[26 - lock binario, lock a 2 fasi#locking a 2 fasi|locking a 2 fasil]] risolve il problema dell’aggregato non corretto, ma non quello della lettura di un dato sporco
>[!info] per risolvere il problema della lettura di dati sporchi occorre che le transazioni obbediscano a regole più restrittive del protocollo di locking a 2 fasi
# locking a 2 fasi stretto
una transazione soddisfa il **protocollo di locking a 2 fasi stretto** se:
1. non scrive sulla BD fino a quando non ha raggiunto il suo punto di commit ( in questo modo, se una transazione è stata abortita, allora non ha modificato alcun item)
2. non rilascia un lock finchè non ha finito di scrivere sulla BD