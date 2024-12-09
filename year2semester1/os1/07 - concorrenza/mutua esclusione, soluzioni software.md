---
created: 2024-12-09
related to: 
updated: 2024-12-09T20:10
---
proviamo ora a gestire la mutua esclusione senza aiuto dal parte dell’hardware o dal SO. gestiremo quindi tutto nel codice (senza la sicurezza di avere operazioni atomiche).
>[!important] le soluzioni che vedremo valgono per 2 processi
>fare il passaggio a $n$ processi è possibile ma non semplice
# code-based
>[!info] primo tentativo
>![[Pasted image 20241209102943.png]]
>- lame, se un processo è da solo e vuole accedere alla sezione critica non lo può fare. L
>- busy-waiting !! L

>[!info] secondo tentativo
>![[Pasted image 20241209103106.png]]
uso un array (di dimensione 2, sempre per 2 processi), per indicare se uno dei 2 processi è in sezione critica. P1 legge il valore del flag di P2 e scrive il suo valore, e P2 legge il valore del flag di P1 e scrive il suo valore.
>- all’inizio entrambi i valori di flag sono inizializzati a 0, quindi se il dispatcher gestisce con interleaving perfetto, entrambi i processi entrano in sezione critica

>[!info] terzo tentativo
![[Pasted image 20241209103438.png]]
uso il flag per comunicare l’intenzione di voler accedere in sezione critica
>- con interleaving perfetto, deadlock comunque. 

>[!info] quarto tentativo
>![[Pasted image 20241209103538.png]]
>l’idea è che con interleaving perfetto, entro nel loop e usando un buon delay, riesco a settare il mio flag a true prima che l’altro processo possa fare lo stesso.
>potrebbe funzionare con buona probabilità, ma non generale abbastanza.
>livelock:
>![[Pasted image 20241209105858.png|250]]

## algoritmo di Dekker
>[!info] algoritmo
>![[Pasted image 20241209110104.png]]
>- `turn`  può essere inizializzato a 0 o 1, non importa
>- se il dispatcher è fair, questa soluzione funziona ! 
>- garantisce il **bounded-waiting**: un processo può aspettare l’altro al più una volta
>- garantisce il non-deadlock, ma è busy-waiting
>- non richiede nessun supporto dal SO(c'è da dire che ci sono alcune architetture moderne che hanno ottimizzazioni hardware che riordinano le istruzioni da eseguire, nonchè gli access in memoria. È NECESSARIO DISABILITARE TALI OTTIMIZZAZIONI !!!)
## algoritmo di Peterson
>[!info] algoritmo
![[Pasted image 20241209110739.png]]
very cool. it works.
>- stesse caratteristiche dell’algoritmo di Dekker !

# passaggio di messaggi
quando un processo interagisce con un altro, due requisiti fondamentali devono essere soddisfatti: 
- sincronizzazione
- comunicazione
il **message passing** (scambio di messaggi) è una soluzione al secondo requisito !
- funziona sia con memoria condivisa che distribuita (i cluster(memoria distribuita) spesso funzionano usando message passing), e può essere usata anche per la sincronizzazione
funziona attraverso 2 primitive:
- `send(destination, message)` (`message` e effettivamente il messaggio da mandare)
- `receive(source, message)` (`message` è la zona di memoria in cui vogliamo ricevere il messaggio)
- inoltre spesso c’è anche il test di ricezione
chiaramente, la comunicazione richiede anche la sincronizzazione (il mittente deve inviare, prima che il ricevente possa leggere)

>[!important] `send` e `receive` sono SEMPRE ATOMICHE
>ciò è garantito dal SO
>- solo un processo per volta esegue una delle due operazioni

- le operazioni di `send` e `receive` possono essere bloccanti oppure no (con bloccanti si intende che mandano il processo che le chiama in `BLOCKED`, per evitare il busy-waiting)
	- il test di ricezione non è mai bloccante
### send e receive bloccanti
se `send` è bloccante, il processo che invia il messaggio diventerà `BLOCKED` fino a che qualcuno non effettuerà un `receive`, e allo stesso modo un processo che chiama `receive` prima di avere un messaggio da ricevere sara `BLOCKED` fino a che non riceve un messaggio
- quindi che un processo si blocchi o non si blocchi dipende da qualche funzione è stata chiamata prima
- tipicamente questo tipo di comunicazione viene chiamato **rendezvous** (ah, l’amore, le donne francesi. they all smell like freshly baked, buttery croissants. dont get me started on their taste)
### send non bloccante
più naturale per molti programmi concorrenti ! la chiameremo `nbsend`. 
- di solito, la ricezione è bloccante
	- in questo modo, il mittente continua, mentre il destinatario rimante `BLOCKED` finchè non ha ricevuto il messaggio
- però la ricezione può esser anche non bloccante (`nbreceive`). per sapere se abbiamo ricevuto un messagio o no (visto che il processo che chiama `nbreceive`, anche se non riceve il messaggio, va avanti lo stesso), la funzione può settare un bit dentro il messagio (la zona di memoria dedicata ad esso ?)per dire se la recezione è avvenuta oppure no (altrimenti può anche essere la return value della funzione)
	- se la ricezione non è bloccante, allora tipicamente non lo è neanche l’invio
## indirizzamento
il mittente deve poter dire a quale processo (o quale gruppo di processi) vuole mandare il messaggio, e lo stesso deve essere possibile per il destinatario (anche se non sempre !)
ogni processo ha una sua coda, che contiene i messaggi ancora non ricevuti(con una `receive`). una volta piena, solitamente il messaggio si perde o viene ritrasmesso (es: syscall `listen` di Linux). ciò è gestito dal SO (dimensione della coda, eventuale ritrasmissione)
si possono usare: 
**indirizzamento diretto**:
- `send` include uno specifico identificatore per il destinatario (o per il gruppo di destinatari)
- per la receive, l’identificatore ci può essere oppure no:
		- `receive(sender, msg)`: ricevi solo se il mittente coincide con `sender` (utile per applicazioni fortemente cooperative)
	- `receive(null, msg)`: ricevi da chiunque(dentro `msg`, c’è anche il mittente)
**indirizzamento indiretto**:
- i messaggi sono inviati ad una particolare zona di memoria condivisa(**mailbox**), che va esplicitamente creata da qualche processo
- il mittente manda messaggi alla mailbox, il destinatario se li va a prendere nella mailbox
>[!example] esempio, caso limite
> può capitare che, con una ricezione **bloccante**: tanti processi chiamino `receive` su una mailbox vuota, e tali processi diventino `BLOCKED`. quando viene mandato un messaggio nella mailbox, **solo uno** dei processi `BLOCKED` diventa `READY`. e gli altri ????

- evidenti analogie con producer/consumer
	- in particolare, se la mailbox è piena allora anche `nbsend` si deve bloccare !!