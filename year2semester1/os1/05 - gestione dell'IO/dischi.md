---
created: 2024-11-17
related to: intro
updated: 2024-11-17, 17:09
---
\\TODO index
# HDD
>[!warning] ciò che verrà visto vale per gli HDD, non per gli SDD (hanno problemi diversi, non trattati in questo corso)
anche se vengono fatti dei cenni sugli SDD 

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

>[!figure] prospetto
![[Pasted image 20241117182258.png]]
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
- sceglie la richiesta che minimizza il movimento della testina (dalla posizione attuale)
- possibile la starvation di richieste! ( se arrivano continuamente richieste più vicine)
>[!figure] minimo tempo di servizio graph
![[Pasted image 20241117175826.png]]
## SCAN
- si scelgono le richieste in modo tale che la testina si muova sempre in un verso, e poi torni indietro
- fixa la starvation delle richieste, ma non è equo:
	- favorisce le richieste “ai bordi”: vicine a dove la testina cambia direzione (risolto con CSCAN)
	- potrebbe favorire le richieste appena arrivate (risolto con N-step-SCAN)

>[!figure] SCAN graph
>![[Pasted image 20241117175908.png]]
## C-SCAN
- come SCAN, ma quando la testina fa “marcia indietro”, non si scelgono richieste
- serve richieste solo quando si muove in una delle due direzioni
- quindi i bordi non vengono visitati 2 volte in poco tempo !
>[!figure] C-SCAN graph
![[Pasted image 20241117180000.png]]
## FSCAN
- fixa il problema di possibile favoritismo verso nuove richieste arrivate (che seguono il percorso che sta facendo la testina) della politica SCAN
- usa due code, anzichè una: F(**front**) e R(**rear**)
	- quando SCAN inizia, tutte le richieste sono nella coda F, e l’altra coda R è vuota, e viene riempita con le richieste che arrivano mentre SCAN sta servendo tutta F
	- quando SCAN finisce di servire F, si scambiano F ed $
- in questo modo, ogni nuova richiesta deve aspettare che tutte le precedenti vengano servite, eliminando il favoritismo !
## N-step-SCAN
- generalizzazione di FSCAN a N > 2 code: si accodano le richieste nella i-esima coda, finche non si riempie: si passa poi alla (i+1) mod N
- niente viene aggiunto alla coda attualmente servita
- se N è alto, le prestazioni sono quelle di SCAN (ma con fairness)
- se N=1, allora si usa il FIFO per evitare di essere unfair
##  confronto prestazionale
>[!figure] confrontos
>![[Pasted image 20241117182014.png]]

# cache del disco
spessoo chiamata **page cache**, è buffer in memoria principale che contiene una copia di alcuni settori del disco:
- quando si fa una richiesta di I/O per dati che si trovano su un certo settore, si vede prima se tale settore è nella cache
- se il settore non c’è, il settore letto viene anche copiato nella cache
- è quindi una cache software non hardware
>[!warning] da non confondere con la cache spesso presente direttamente sui dischi !
>una cache hardware che è quindi trasparente al SO

ci sono svariati modi per gestire questa cache:
## politiche di rimpiazzo
### LRU
se occorre rimpiazzare qualche settore nella cache piena, si prende quello nella cache da più tempo senza referenze
- la cache viene costruita con uno “stack” di puntatori che puntano ai diversi settori: il settore riferito più recentemente sarà quindi in cima allo stack, e quello riferito meno recentemente sarà in fondo allo stack. (anche se la cache non è propriamente uno stack, in quanto NON viene acceduta utilizzando le funzioni push, pop e top in quanto si poppa anche dal basso)
### LFU
si rimpiazza il settore con meno frequenze (quindi, il settore che è stato riferito meno di recente potrebbe non essere quello con meno frequenza)
- LRU si basa sul tempo, mentre LFU sulla frequenza !
- serve un contatore per ogni settore(inizialmente 1, incrementato ad ogni riferimento
- può sembrare intuitivamente corretto: meno vieni usato, meno servi. però, di solito un settore viene acceduto svariate volte di fila, perchè contiene dati acceduti secondo il principio di località. dopoichè, però, non serve più, e andrebbe quindi sostituto. invece, secondo LFU, esso avrà una frequenza abbastanza alta
### sostituzione basata su frequenza
- viene usato uno stack di puntatori come LRU, ma è spezzato in 2: una parte nuova, e una parte vecchia
- si rimpiazza comunque il settore con il contatore di frequenza più basso, ma solo tra i puntatori nella parte vecchia (quindi un settore meno usato, con meno riferimenti)
- c’è ancora un problema: può capitare che un settore appena inserito venga subito rimosso 
# SSD
\\TODO
