---
created: 2024-12-13
related to: 
updated: 2024-12-13T10:30
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
## ricerca
per ricercare un record con valore della chiave $k$, occorre cercare sul file indice un valore $k’$ della chiave che **ricopre $k$**, cioè tale che:
- $k' \leq k$
- se il record $k’$ non è l’ultimo record del file indice e $k’’$ è il valore della chiave nel record successivo, $k < k''$
la ricerca di un record con chiave $k$ richiede:
- 1 ricerca sul file indice ($n$ accessi per caricare l’$n$ esimo blocco, che contiene $k’$)
- 1 accesso in lettura sul file principale
## ricerca binaria
poichè il file indice è ordinato in base al valore della chiave, la ricerca di un valore che ricopre la chiave può essere fatta in modo efficiente mediante la **ricerca binaria**
la ricerca è fatta in questo modo:
$m$ è il numero totale di blocchi per il file indice
- si fa un accesso in lettura al blocco $\left( \frac{m}{2} \right)+1$ e si confronta $k$ con $k1$(la prima chiave del blocco)
	- se $k = k1$ abbiamo finito!
	- se $k < k1$ allora si ripete il procedimento sui blocco i da $1$ a $(\frac{m}{2})$
	- altrimenti si ripete il procedimento sui blocchi da $\left( \frac{m}{2} \right)+1$ ad $m$ (il blocco $\left( \frac{m}{2} \right)+1$ va riconsiderato, perchè abbiamo controllato solo la prima chiave !)
- ci si ferma quando lo spazio di ricerca è ridotto ad un unico blocco, quindi dopo $[\log_2 a]$ accessi
>[!info] ricerca sveglia che puoi fare 
se $k>k1$, prima di ripetere la ricerca binaria, controlliamo tutti i valori del blocco che abbiamo caricato (che contiene $k1$).
>- se troviamo la chiave che copre $k$, abbiamo finito.
>- però, se $k>km$, con $km$ l’ultima chiave del blocco, non possiamo sapere se $k$ è coperta da $km$ o se è coperta da una chiave nei blocchi successivi. quindi si deve continuare con la ricerca binaria
## ricerca per interpolazione sul file indice
la ricerca per interpolazione è basata sulla conoscenza della distribuzione dei valori della chiave:
- deve essere disponibile una funzione $f$, che dati tre valori $k1, k2, k3$ della chiave, fornisce un valore che è la frazione dell’intervallo di valori della chiave compresi tra $k2$ e $k3$ in cui deve trovarsi $k1$($k1$ chiave di ricerca)
	- nella ricerca binaria, questa frazione è sempre $\frac{1}{2}$
la ricerca per interpolazione richiede circa $1 + \log_2 \log_2 m$ accessi, rendendola quindi molto più veloce, ma è molto difficile conoscere $f$ e poi la distribuzione dei dati potrebbe cambiare nel tempo
// non ci siamo soffermati molto su questa ricerca se ricordo bene
## inserimento
esistono diverse situazioni che influenzano il costo dell’insierimento di un record.
l’ inserimento richiede
	- costo della ricerca
	- 1 accesso per scrivere il blocco modificato(c’è spazio nel blocco)
	- altrimenti, 1 accesso per leggere il blocco in cui non c’è spazio, 1 accesso per scrivere il blocco successivo (in cui troviamo spazio) 
se non c’è spazio nè nel blocco precedente nè nel successivo, occorre richiedere un nuovo blocco al file system, ripartire i record tra il vecchio e il nuovo blocco(quando alloco un nuovo blocco, non metto un singolo record, ma ne metto diversi, per lasciare dello spazio libero) e riscrivere tutti i blocchi modificati
>[!example]- esempio di inserimento con richiesta di nuovo blocco
>![[Pasted image 20241213102439.png]]
>![[Pasted image 20241213102507.png]]
>![[Pasted image 20241213102541.png]]
## cancellazione
la cancellazione richiede:
- costo della ricerca
- 1 accesso per scrivere il blocco modificato, e in più:
	- se il record cancellato è il primo di un blocco, va modificato anche l’indice (1 accesso in scrittura)
	- se il record cancellato è l’unico del blocco, il blocco viene restituito al sistema e viene modificato anche l’indice (1 accesso in scrittura)
## modifica
la modifica richiede:
- costo della ricerca
- 1 accesso in scrittura sul blocco modificato
>[!important] se la modifica coinvolge lal chiave, la modifica diventa cancellazione + inserimento
