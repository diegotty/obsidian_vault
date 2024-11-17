---
created: 2024-11-17
related to: intro
updated: 2024-11-17, 17:09
---
\\TODO index
>[!warning] ciò che verrà visto vale per gli HDD, non per gli SDD (hanno problemi diversi, non trattati in questo corso)

>[!info] costruzione del disco
![[Pasted image 20241117171021.png]]
>- i dischi sono divisi in tracce, che sono divise da spazi vuoti
>- ogni traccia è diviso in settori
>- nelle tracce, i settori sono divisi da degli spazi vuoti

per leggere/scrivere dati, serve sapere su quale traccia si trovano i dati, e per selezionare una traccia bisogna:
- spostare una testina, se il disco ha testine mobili (quindi magari ha una sola testina)
- selezionare una testina, se il disco ha testine fisse (quindi ha più testine)
per selezionare il settore invece, bisogna aspettare che il disco ruoti ( a velocità costante)
se i dati sono tanti, potrebbero essere su più settori o addirittura più tracce !! 
- la tipica misura di un settore è 512 bytes

## prestazioni del disco
>[!info] fasi del trasferimento
![[Pasted image 20241117173234.png]]
>- esistono dischi con più canali (hanno più dischi fisici, e bisogna quindi anche selezionare il disco: wait for channel)

il tempo di accesso (**access time**) è quindi la somma di:
- **seek time**: tempo necessario perchè la testina si posizioni sulla traccia desiderata
- **rotational delay**: tempo necessario affinchè l’inizio del settore raggiunga la testina
- **transfer time**: tempo necessario a trasferire di dati che scorrono sotto la testina
a parte (non fanno parte delle effettive prestazioni del disco):
- **wait for device**: attesa che il dispositivo sia assegnato alla richiesta
- **wait for channel**: attesa che il sottodispositivo sia assegnato alla richiesta
	- se ci sono più dischi che condividono un unico canale di comunicazione

# politiche di scheduling per il disco
come nella RAM, sono stati pensati diversi algoritmi per ottimizzare lo scheduling delle richieste di trasferimento per i dischi
per confrontare i vari algoritmi, usiamo un esempio comune:
>[!example] esempio comune
>- all’inizio, la testina si trova sulla traccia numero 100
>- ci sono 200 tracce
>- vengono richieste le seguenti tracce nel seguente ordine:
>$$55, 58, 39, 18, 90, 160, 150, 38, 184$$
>- considereremo solo il **seek time**, che è il parametro più importante per le prestazioni
>- il nostro scheduling baseline sarà il random, che è lo scheduling peggiore
## FIFO
- le richieste vengono servite in modo sequenziale, rendendo lo scheduling equo nei confronti dei processi
- però, se ci sono molti processi in esecuzione, le prestazioni sono simili allo scheduling random
>[!figure] FIFO graph
![[Pasted image 20241117174523.png]]
## priorità
- l’obiettivo non è ottimizzare il disco, ma rispettare le priorità
- impossibile fare il grafico in quanto manca l’infomazione sulla prorità di chi ha richiesto le informazioni
## LIFO
- ottimo per DBMS con transazioni (sequenze di richieste che vanno eseguite tutte insieme)
- LIFO si riferisce all’utente! il dispositivo viene dato all’utente più recente (quindi è possibile la starvation per gli utenti !)
- motivazione: se si tratta dello stesso utente, probabilmente sta accedendo sequenzialmente ad un file, e quindi è più efficiente mandare avanti lui (e bisogna far finire la transazione)
impossibile fare il grafico perchè non sappiamo quale utente fa la richiesta
## minimo tempo di servizio
>[!warning] da qui in poi occorre conoscere la posizione della testina
- sceglie la richiesta che minimizza il movimento del 