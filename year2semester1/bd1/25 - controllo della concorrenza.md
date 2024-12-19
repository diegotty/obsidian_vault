---
created: 2024-12-19
related to: "[[intro alla concorrenza]]"
updated: 2024-12-19T09:32
---
in sistemi di calcolo con una sola CPU, i programmi sono eseguiti concorrentemente in modo **interleaved**: la CPU esegue alcune istruzioni di un programma, sospende quel programma, esegue istruzioni di altri programmi, ritorna ad eseguire istruzioni del primo, etc.
- questo perchè l’esecuzione concorrente permette un uso efficiente della CPU
in un DBMS la principale risorsa a cui i programmi accedono in modo concorrente è la base di dati
- se sulla BD vengono effettuate **letture** (la BD non viene mai modificata), l’accesso concorrente non crea problemi
- se sulla BD vengono effettuate anche **scritture** (la BD viene modificata), l’accesso concorrente può creare problemi e deve essere quindi **controllato**
# transazione
una transazione è l’esecuzione di una parte di programma che rappresenta un’unità logica di acceso o modifica del contenuto della base di dati

## proprietà della transazioni
le transazioni sono dotate di proprietà **ACID** (Atomicity, Consistency, Isolation, Durability)

- **atomicità**: una transazione è **indivisibile** nella sua esecuzione, e la sua esecuzione deve essere **o totale, o nulla**, non sono ammesse esecuzioni parziali.

- **consistenza**: quando inizia una transazione, la BD si trova in uno stato consistente, e quando la transazione termina la BD deve **essere in un altro stato consistente**, ovver non deve violare eventuali vingoli di integrità, quindi non devono verificarsi contraddizioni (inconsistenza) tra i dati archiviati nella BD
 
 - **isolamento**: ogni transazione deve essere eseguita in modo isolato ed indipendente dalle altre transazioni, e l’eventuale fallimeno di una transazione non deve interferire con le altre transazioni in esecuzione
	 - una transazione ben progettata non deve dipendere da altre transazioni (il risultato non deve dipendere dal fatto che altre transazioni vengano eseguite prima o dopo (il risultato può cambiare, )) se ho 3 transazioni $T_1,T_2, T_3$, qualunque permutazione delle transazioni deve essere eseguibile
 
 - **durabilità**: anche chiamata persistenza, si riferisce al fatto che una volta che una transazione abbia richiesto un **commit work**, i cambiamenti apportati non dovranno essere più persi. per evitare che nel lasso di tempo fra il momento in cui la base di dati si impegna a scrivere le modifiche, e quello in cui le modifiche vengono effettivamente scritte si verifichino perdite di dati dovuti a malfunzionamenti, **vengono tenuti dei registri di log** dove sono annotate tutte le operazioni sulla BD
# schedule
uno schedule è un piano di esecuzione di un insieme $T$ di transizioni
lo schedule è un ordinamento **delle operazioni delle transazioni** in $T$ che conserva l’ordine che le operazioni hanno all’interno delle singole transazioni
>[!example] esempio
>se l’operazione $O1$ precede l’operazione $O2$ nella transazione $T$, sarà così anche in ogni schedule in cui compare $T$ (le due operazioni saranno eventualmente separate da operazioni di altre transazioni, ma il loro ordine non verrà mai invertito)
## schedule seriale
schedule ottenuto permutando le transazioni di $T$. uno schedule seriale corrisponde ad una esecuzione **sequenziale**(senza interlaving) delle transazioni
- una transazione che prende la CPU non la rilascia finchè non ha finito le sue operazioni
- lo schedule seriale è il nostro punto di riferimento
## problemi
>[!important] quando una transazione legge un item, esso viene preso dalla memoria secondaria e caricato in una parte privata, della singola transazione, di memoria principale

quali sono i problemi che possono sorgere a causa dell’esecuzione concorrente dei programmi ?
>[!example] dati
>date le transazioni $T_1, T_2$:
![[Pasted image 20241219093135.png]]

>[!example] esempio 1: ghost update
![[Pasted image 20241219093217.png]]
in questo schedule, l’aggiornamento di $X$ prodotto da $T1$