---
created: 2024-12-19
related to: 
updated: 2024-12-19T09:18
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
- **consistenza**: quand
 - **isolamento**: una transazione ben progettata non deve dipendere da altre transazioni (il risultato non deve dipendere dal fatto che altre transazioni vengano eseguite prima o dopo (il risultato può cambiare, )) se ho 3 transazioni $t_1,t_2, t_3$, qualunque permutazione delle transazioni deve essere eseguibile. ogni transazione deve essere indipendente !
 - **durabilità**
