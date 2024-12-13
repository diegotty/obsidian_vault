---
created: 2024-12-13
related to: 
updated: 2024-12-13T09:12
---
quando le chiavi ammettono un ordinamento significativo per l’applicazione, e più conveniente utilizzare un’organizzazione fisica dei dati che ne tenga conto
- interi e stringhe ammettono i consueti ordinamenti (lessicografico per le stringhe)
- se la chiave ha campi multipli, si ordina sul primo campo, poi sul secondo, e così via
>[!important] le chiavi possono sempre essere ordinate !
## ordine lessicografico
un alfabeto finito **totalmente ordinato** di simboli è un insieme $\sum = ( \delta_{1}, \delta_{2}, \dots, \delta_{n})$ dotato di un ordine totale (relazione d’ordine totale !!! mmi) $\delta_{1} < \delta_{2}< \dots<\delta_{n}$

date due sequenze di simboli $$I = \delta_{i1} \,\delta_{i2} \dots \delta_{in} \,\,\,\text{e} \,\,\,\,J = \delta_{j1} \, \delta_{j2}\dots\delta_{jm}$$
diciamo che $I < J$ se esiste un numero $k \in \mathbb{N}$ per cui $$\delta_{i1} \,\delta_{i2} \dots \delta_{ik} = \delta_{j1} \,\delta_{j2} \dots \delta_{jk}$$e vale una delle seguenti relazioni:
$$\delta_{i(k+1)} < \delta_{j(k+1)} \,\,\text{oppure} \,\, n = k < m$$
(quindi, se $I$ è più corta di $J$, $I < J$)

# file con indice (sparso)
## file ISAM
**ISAM**: indexed sequential access method
>[!important] caratteristica principale !
file principale ordinato + indice

il file principale (la tabella del nostro DB) viene ordinato in base al valore della **chiave** di ricerca, che in questo caso è univoca (e chiamiamo primary key)
- in genere viene lasciata una certa percentuale di spazio libero ogni blocco, per facilitare operazioni di inserimento, cancellazione e modifica che potrebbero occorrere
viene inoltre creato un nuovo file: il **file indice**, che contiene un record per ogni blocco del file principale, e ogni record del file indice ha 2 campi:
- un puntatore ad un blocco del file principale
- il più piccolo valore della chiave presente nel blocco (il valore della chiave nel primo record del blocco a cui punta, visto che il file principale è ordinato)
	- il valore delle chiavi nell’indice è strettamente maggiore del valore del record precedente nel file ( e strettamente minore del successivo, se presente), e in questo modo viene rispettata la **proprietà di copertura** 
>[!warning] il valore del primo record del primo blocco del file indice è $- \infty$
e il puntatore punta al primo blocco del file principale
>- non è necessario mettere un valore, in quanto sapremo che dovremo seguire il puntatore del primo record se la chiave di ricerca è $<$ del valore del secondo record dell’indice

>[!info] rappesentazione
![[Pasted image 20241213090504.png|350]]
