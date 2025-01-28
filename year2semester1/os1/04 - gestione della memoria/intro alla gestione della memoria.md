---
created: 2024-10-27
related to: 
updated: 2025-01-28T17:32
---
>[!index]
>
>- [requisiti](#requisiti)
>	- [rilocazione](#rilocazione)
>		- [tipi di indirizzi](#tipi%20di%20indirizzi)
>		- [preprocessing](#preprocessing)
>		- [run-time](#run-time)
>		- [registri usati per la rilocazione di indirizzi relativi](#registri%20usati%20per%20la%20rilocazione%20di%20indirizzi%20relativi)
>	- [protezione](#protezione)
>	- [condivisione](#condivisione)
>	- [organizzazione logica](#organizzazione%20logica)
>	- [organizzazione fisica](#organizzazione%20fisica)
# gestione della memoria RAM
anche se la memoria è oggi a basso costo, le applicazioni moderne richiedono sempre maggiore memoria. se si lasciasse a ciascun processo la gestione della propria memoria, ogni processo userebbe semplicemente tutta la memoria a disposizione
- in questo modo non ci sarebbe più multiprogrammazione, che però è essenziale
- si potrebbe imporre dei limiti di memoria per ogni programma, ma in questo modo diventa difficile per un programmatore scrivere un processo che rispetta tali limiti
“l’unica” soluzione è quindi che il sistema operativo si occupi della memoria:
- i processi fanno finta di avere tutta la memoria a loro disposizione, e sta al SO rendere realistica l’illusione
- uso il disco come area di memoria secondaria, come buffer per i processi
- dato che l’uso di I/O è ovviamente più lento del processore, il SO deve pianificare lo swap(spostamento da ram a disco, e viceversa) in modo intelligente, così da massimizzare l’efficienza del processore
- ci devono essere sempre un numero ragionevole di processi pronti all’esecuzione(nella RAM), così da non lasciare inoperoso il processore
# requisiti
## rilocazione
dato che un processo può essere swappato su disco, al suo ritorno in memoria principale, potrebbe essere rilocato ad un’altra parte della RAM, diversa da dove si trovava prima dello swap, e magari con alcune pagine in RAM e altre su disco
il programmatore non sa e non deve sapere in quale zona della memoria il programma verrà caricato, e il processo deve poter andare in esecuzione sia che sia caricato all’inizio della memoria che alla fine
- i riferimenti in memoria usati nelle istruzioni macchina(quando si programma in assembly o quando viene compilato un programma) usati nel programma devono essere tradotti nell’indirizzo fisico “vero”
	- ciò si può fare in 2 modi: preprocessing o run-time(che richiede supporto hardware)
>[!info] esempio di compilazione
![[Pasted image 20241023080141.png]]
il linker genera un load module, che può essere caricato dal loader nella memoria principale

>[!info] esempio di moduli diversi
![[Pasted image 20241023080709.png]]
nel modulo (b), se il processo non viene caricato all’indirizzo 1024, il jump salta nella parte sbagliata
nel modulo (c), i valori dei jump\load indicano di quanto si deve saltare (in modo relativo), infatti quando vengono caricati nella memoria principale (modulo (d)), aggiunge l’offset relativo all’indirizzo di memoria in cui è stato caricato il processo
### tipi di indirizzi
- logici: riferimento in memoria è indipendente dall’attuale posizionamento del programma in memoria (quelli che si trovano nel codice)
- relativi: riferimenti espressi come spiazzamento rispetto ad un qualche punto noto (caso particolare degli indirizzi logici)
- fisici\assoluti: riferimenti effettivi alla memoria
### preprocessing
vecchia soluzione: gli indirizzi assoluti vengono determinati nel momento in cui il programma viene caricato in memoria(nuovamente o per la prima volta)
- si può fare senza hardware dedicato
-  nel CTSS ciò è reso ancora più facile, i programmi partono sempre dall’indirizzo 5000, quindi si può usare l’absolute load module (b)
### run-time
soluzione più recente: gli indirizzi assoluti vengono determinati nel momento in cui si fa un riferimento alla memoria 
- serve hardware dedicato
>[!figure] ![[Pasted image 20241023082643.png]]
il SO si occupa di mettere il valore giusto nel Base Register, in questo modo il processo può essere spostato in qualunque zona della RAM
il risultato dell’absolute address viene confrontato con il bounds register, e se l’absolute address va oltre, viene generato un interrupt per il SO (tipo segmentation fault)
### registri usati per la rilocazione di indirizzi relativi
- base register: indirizzo di partenza del processo
- bounds register: indirizzo di fine del processo
i valori di questi registri vengono settati nel momento in cui il processo viene posizionato in memoria, e sono manenuti nel PCB del processo.
- il set fa parte del passo 6 del [[gestione dei processi#switching tra processi]]
## protezione
i processi non devono poter accedere a locazioni di memoria di un altro processo, a meno che non siano autorizzati
- a causa delle rilocazione, non si può fare a tempo di compilazione, bisogna gestirlo a tempo di esecuzione, con aiuto hardware
## condivisione
la maggio parte dell’immagine del processo deve essere protetta, ma deve essere possibile permettere a più processi di accedere alla stessa zona di memoria
>[!example] quando più processi vengono creati eseguendo più lo stesso codice sorgente, è più efficiente che condividano il codice sorgente, visto che è lo stesso

ci sono anche casi in cui processi diversi vengono esplicitamente programmati per accedere a sezioni di memoria comuni, usando syscall 
- questo è anche il caso di thread di uno stesso processo
## organizzazione logica
a livello hardware, la memoria è organizzata in modo lineare, mentre a livello software non viene utilizzata in modo lineare ! 
- i programmi sono scritti in moduli, che possono essere scritti e compilati separatamente, a cui possono essere dati diversi permessi (sola lettura, sola esecuzione), e che possono essere condivisi tra processi
il SO deve fare da ponte tra la visuale a livello HW e la visuale a livello SW (spesso tramite segmentazione)
## organizzazione fisica
gestisce dell flusso tra RAM e memoria secondaria, ed è gestita da SO