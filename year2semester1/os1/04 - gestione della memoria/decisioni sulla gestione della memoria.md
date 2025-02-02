---
created: 2024-11-03
related to: 
updated: 2025-02-02T21:18
---
>[!index]
>
>- [dimensione delle pagine](#dimensione%20delle%20pagine)
>- [elementi centrali per il progetto del SO](#elementi%20centrali%20per%20il%20progetto%20del%20SO)
>- [fetch policy](#fetch%20policy)
>- [placement policy](#placement%20policy)
>- [replacement policy](#replacement%20policy)
>- [gestione del resident set](#gestione%20del%20resident%20set)
>	- [frame locking](#frame%20locking)
>- [politica di pulitura](#politica%20di%20pulitura)
>- [medium term scheduler](#medium%20term%20scheduler)
>- [controllo del carico](#controllo%20del%20carico)
>- [algoritmi di sostituzione](#algoritmi%20di%20sostituzione)
>- [buffering delle pagine](#buffering%20delle%20pagine)
>- [gestione della memoria in Linux](#gestione%20della%20memoria%20in%20Linux)
>- [gestione della memoria kernel](#gestione%20della%20memoria%20kernel)
>- [gestione memoria utente](#gestione%20memoria%20utente)
>	- [replacement policy](#replacement%20policy)
# caratteristiche
## dimensione delle pagine
pagine piccole:
- minore frammentazione(che può avvenire solo nel frame dell’ultima pagina per ogni processo)
- maggior numero di pagine per processo, quindi maggiore dimensione della PT
- la maggior parte delle PT finisce in memoria secondaria
- maggior numero di pagine nella RAM, quindi maggiore multiprogrammazione
- resident set per processo più grande, meno page fault
pagine grandi:
- la memoria secondaria è ottimizzata per trasferire grossi blocchi di dati, quindi avere le pagine ragionevolmente grandi conviene
>[!example] page faults vs dimensione pagine
![[Pasted image 20241101184446.png]]
>- **nel primo grafico**, si può notare che il maggior numero di page fault avviene quando le pagine sono circa metà della dimensione del processo. inoltre, nella parte di grafico dopo il picco, avvicinandosi alla dimensione delle pagine pari alla dimensione del processo stesso, non si denota dal grafico che la multiprogrammazione diminuisce. è quindi meglio avere pagine piccole, poichè i page fault non ne risentono e la multiprogrammazione è più alta
> - **nel secondo grafico** si nota lo “sweet spot” per la cardinalità del **resident set**
# elementi centrali per il progetto del SO
- fetch policy
- placement policy
- replacement policy
- gestione del resident set
- politica di pulitura
- controllo del carico
il tutto, cercando di minimizzare i page fault; non c’è una politica sempre vincente
## fetch policy
decide quando una pagina debba essere portata in RAM
si usano principalmente due politiche:
- paginazione su richiesta (demand paging): quando una pagina richiede una cella di memoria che sta in una pagina che non è caricata nella memoria principale, essa viene presa dal disco e caricata in RAM
	- usando questa politica, all’inizio del processo ci saranno molti page fault, in quanto in RAM sarà presente solo la prima pagina
- prepaginazione (prepaging): cerca di anticipare le necessità del processo, portano in RAM più pagine di quelle richieste (sfruttando località e la capacità dei dischi moderni (dischi ottimizzati per trasferimento di grandi blocchi))
## placement policy
decide dove mettere una pagina in memoria principale quando c’è almeno un frame libero ( se non ci sono frame liberi, si una la replacement policy !)
dato che c’è l’hardware per la traduzione degli indirizzi, una pagina può essere messa ovunque
- tipicamente; il primo frame libero (quello con l’indirizzo più basso, è quello dove viene messa la pagina)
## replacement policy
decide cosa fare se occorre portare una pagina in RAM, e tutti i frame sono già occupati
in questa situazione qualche pagina va sostituita, sovrascrivendo il rispettivo frame
per gesitre ciò esistono vari agoritmi (replacement policy), in quanto bisogna minimizzare la probabilià che la pagina appena sostituita venga richiesta di nuovo subito dopo
- quando viene swappata una pagina, bisogna aggiornare la sua PTE !! (in particolare, il bit M)
### algoritmi di sostituzione
**soluzione ottimale**
- si sostiuisce la pagina che verrà richiesta a più distanza (tenendo quindi le pagine che serviranno nel futuro più prossimo)
- ovviamente non è implementabile, però è ben definibile sperimentalmente e viene usata come confronto per gli altri algoritmi
>[! example] esempio soluzione ottimale
>![[Pasted image 20241102120457.png]]
>- primo fault:sostituisco la pagina 1 perchè so che non verrà più richiesta in futuro
>- secondo fault: sostituisco la pagina 2 perchè è quella che verrà chiamata più tardi in futuro 
>- terzo fault: sostituisco la pagina 4 perchè le pagine 5 e 2 verrano chiamate in futuro

**sostituzione LRU**
- sostituisce la pagina a cui non è stato fatto riferimento per il tempo più lungo, che, per il principio di località, dovrebbe essere la pagina che ha meno probabilità di essere usata nel futuro
- l’implementazione è problematica: bisogna etichettare ogni frame con il tempo dell'ultimo accesso, e mentre la cache usa questa tecnica perchè è implementata in hardware, per la quantità di frame della RAM è troppo costoso
>[!example] esempio soluzione LRU
![[Pasted image 20241102121334.png]]
> - primo fault: l’LRU pensa che la pagina uno verrà usata in futuro (in quanto quella con l’ultimo accesso), è invece la pagina che non verrà più usata

**sostituzione FIFO**
- proviamo a rendere l’overhead più basso possibile !
- viene usato un puntatore, che tratta i frame come una coda circolare, e si sposta ogni volta che viene modificato un frame (si sposta al frame successivo). ogni volta che bisogna fare un rimpiazzo, viene sostituita la pagina nel frame puntato dal puntare (che è frame che contiene la pagina presente da più tempo)
- implementazione semplice
>[!example] esempio sostituzione FIFO
![[Pasted image 20241102122438.png]]
>- dopo la quarta richiesta di pagina, il puntatore si sposta al frame 1 (contente la pagina 2)
>- troppi page fault !!!

**sostituzione dell’orologio**
l’algoritmo usato attualmente (+ o -, e con variazioni)
- compromesso tra LRU e FIFO: esiste il puntatore usato nella soluzione FIFO, ma viene aggiunto un “**use bit**” per ogni frame, che indica se la pagina è stata riferita di recente
- il bit viene settato ad 1 quando la pagina viene caricata in RAM, e poi rimessa ad uno per ogni accesso al suo interno.
- quando occorre sostituire una pagina, il SO guarda il puntatore che si comporta come nella soluzione FIFO, ma selezione il frame contenente la pagina che ha per per prima lo use bit a 0
- se invece il puntatore punta ad una pagina che la lo use bit ad 1, lo setta a 0 e si sposta avanti
>[!example] sostituzione dell’orologio
![[Pasted image 20241102123028.png]]
>- primo fault: dopo la quarta richiesta, il puntatore è sul frame 1. quando arriva la richiesta 5 e bisogna effettuare un rimpiazzo, viene controllato il frame su cui si troava il puntatore. lo use bit è 1, quindi lo use bit del frame 1 viene messo a 0, e il puntatore passa al frame 2. il caso è lo stesso, e lo use bit del frame 2 viene messo a 0. succede anche la stessa cosa per il frame 3, e il puntatore si muove avanti sul frame 1 (coda circolare). dato che lo use bit del frame 1 è stato messo a 0, la pagina contenuta viene rimpiazzata con la pagina 5.
>- decima richiesta: lo use bit del frame 2, visto che contiene la pagina necessaria, passa da 0 a 1.

## gestione del resident set
racchiude due problemi:
- **gestione del resident set**: quanti frame di RAM da allocare per ogni processo da eseguire\in esecuzione ( ad alcuni processi potrebbero bastare un paio di frame, ad altri ne potrebbero service centinaia. ciò si gestisce in 2 modi:
	- allocazione fissa: numero di frame deciso al tempo di creazione di un processo
	- allocazione dinamica: numero di frame varia durante la vita del processo, magari basandosi sulle statistiche che a mano a mano vengono raccolte
- **gestione del replacement scope**: scelta, in fase di rimpiazzo di un frame, tra i frame che appartengono al processo corrente **oppure** anche tra i frame di un processo qualsiasi. ciò si gestisce in 2 modi:
	- politica locale: si sceglie solo tra gli altri frame dello stesso processo
	- politica globale: si può scegliere qualsiasi frame (basta che non sia del SO), ciò comporta un calcolo in più !

si nota come, pur avendo 4 politiche da poter combinare (a coppie di 2), si possono avere in tutto 3 possibili strategie, in quanto con l’allocazione fissa, la politica globale non è applicabile, altrimenti si potrebbe ampliare il resident set di un processo scegliendo un frame di un altro processo per il rimpiazzo (e quindi non sarebbe più allocazione fissa)
>[!figure] ![[Pasted image 20241102103108.png]]
>si nota come nel secondo grafico, lo “sweet spot” è W, non N (se carico tutte le pagine di un processo in RAM, la multiprogrammazione diminuisce drasticamente)
### frame locking
si può “bloccare” un frame, cioè, rendere impossibile il rimpiazzamento di esso: se un frame è locked, non si può sostituire !
questa caratteristica è usata a livello di kernel del SO, e si gestisce con un bit ( gestito a livello hardware)
## politica di pulitura
come nella cache, può capitare che una pagina caricata in RAM venga modificata(sempre in RAM). la politica di pulitura gestisce quando riportare la modifica al disco (se riportarla quando avviene la modifica, o quando la pagina viene sostituito )
- si fa tipicamente una via di mezzo (intrecciata con il page buffering), cioè si raccolgono un po’ di richieste di frame da modificare e li si esegue
# medium term scheduler
torniamo al [[intro allo scheduling#medium-term scheduling|medium term scheduling]], con più informazioni riguardo al suo lavoro !
>[!figure] ![[Pasted image 20241102105949.png]]
>si nota come dopo il picco del grafico, l’utilizzo del processore decresce in quanto ci sono troppi page fault e si spende troppo tempo ad aspettare le risposte del disco (per il rimpiazzo)
## controllo del carico
significa cercare di tenere la multiprogrammazione alta, ma senza avere resident sets così bassi da avere troppi page fault
il carico viene controllato aumentando e diminuendo il numero di processi attivi, aumentando la multiprogrammazione ma senza arrivare al thrashing
- cambiando lo stato da `READY` a `READY/SUSPEND` oppure da `BLOCKED` a `BLOCKED/SUSPEND` e viceversa ! (inoltre si cerca di sospendere processi che non possono o in futuro non saranno ready, e di portare in RAM processi che potranno essere eseguiti (es: non stanno aspettando risposte di richieste I/O))
>[!info] un processo è suspended quando il suo resident set è 0

periodicamente, un processo del SO controlla la situazione della multiprogrammazione, e decide se è necessario aumentare o diminuire i processi attivi

# buffering delle pagine
una cache software (uso i frame, quindi sottraggo frame ai processi) usata per le pagine, creata come modifica della soluzione FIFO(e lo avvicina alla soluzione clock !), che però può essere usata anche con LRU e/o clock
le pagine rimpiazzate non vengono subito buttate via, ma spostate in questa cache, in modo che, se viene nuovamente referenziata, la si può subito riportare in memoria
# gestione della memoria in Linux
in Linux c’è una distinzione netta tra le richieste di memoria da parte del kernel e di processi utente
- il kernel si fida di se stesso: pochi (se non nessuno) controlli per richieste da parte di se stesso
- i processi utente sono soggetti a controlli di protezione e di rispetto dei limiti assegnati
inoltre la gestione della memoria è completamente diversa 
## gestione della memoria kernel
>[!warning] il kernel ha una parte di memoria riservata ! e si trova all’inizio della RAM
>però può usare anche la memoria normalmente usata dai processi utente

il kernel può effettuare sia richieste piccole che richieste grandi (dimensione della memoria richiesta), ed è ottimizzato per entrambe:
- se la richiesta è piccola, fa in modo di avere alcune pagine già pronte (**slab allocator**)
- se la richiesta è grande (fino a 4MB), fa in modo di allocare più pagine contigue in frame contigui
	- ciò è importante per il [[ciclo di esecuzione e interruzioni#accessso diretto in memoria|DMA]], che ignora il fatto che ci sia la paginazione e va dritto in RAM	
//TODO fuck fuck fuck

## gestione memoria utente
- fetch policy: paging on demand
- placement policy: il primo frame libero
- gestione del resident set: politica dinamica con replacement scope globale (nello scope rientra anche la cache del disco !)
- politica di pulitura: ritardata il più possibile, e scrive quando:
	- la page cache è troppo piena con molte richieste pending
	- troppo **dirty pages**(pagina modificata soltanto in RAM, e non sul disco), o una pagina sporca da troppo tempo
	- richiesta esplicita di un processo: syscall `flush`, `sync` o simili
- controllo del carico: assente
### replacement policy
studiamo un algoritmo che si basa sull’algoritmo del clock, ma il kernel più recente usa un LRU “corretto”(l’algo del clock era molto efficace ma poco efficiente)
vengono usati 2 flag in ogni PTE: 
- `PG_referenced`, che sono le pagine che sono state appena usate
- `PG_active`, che sono le pagine più riferite, che non vengono considerate per il rimpiazzo
quando una pagina viene swappata in RAM, nasce con `PG_active` = `PG_referenced` =1
le pagine vengono quindi divise in 2 gruppi: **inactive**, se `PG_active`=0, e **active**, se `PG_active`=1
esiste un **kswapd**, un kernel thread in esecuzione periodica, che scorre solo le pagine inattive

solo le pagine inattive possono essere rimpiazzate, e tra di esse si fa una **mezza specie di LRU con contatore a 8 bit** (ceh non viene usata la ““‘data””” dell’ultimo accesso ma viene incrementato (e resettato se la pagina viene riferita) un contatore (e suppongo che 8 bit bastino (il contatore non si resetta per sbaglio, il processo viene swappato prima)))
>[!example] stati della replacement policy
![[Pasted image 20241102142100.png]]
>- per passare da `PG_active`=0 a `PG_active`=1 è necessario che il processo venga usato 2 volte
>- il timeout è il passaggio del kswapd