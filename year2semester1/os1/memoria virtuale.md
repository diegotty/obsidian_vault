---
created: 2024-10-27
related to: "[[partizionamento e paginazione]]"
updated: 2024-10-27, 16:18
---
fino ad ora abbiamo come costanti, attraverso i vari modi di gestire la memoria, che :
- i riferimenti alla memoria avvengono tramite indirizzi logici, che vengono tradotti ad indirizzi fisici a tempo di esecuzione
- un processo può essere spezzato in più parti, che non necessariamente occupano una zona contigua di memoria
se queste due caratteristiche sono vere, allora non è necessario che tutte le pagine(o segmenti) di un processo siano in memoria principale, per far poter eseguire il processo: in RAM servono solamente la prossima istruzione e i dati di cui ha bisogno
grazie a questa intuizione si può cambiare il modo in cui si gestisce la memoria:
- il SO porta in memoria principale solo alcune pagine del processo: queste pagine si chiamano **resident set** del processo
- viene generato un interrupt (**page fault**) quando al processo serve un indirizzo che non si trova in RAM
- visto che l’interrupt lanciato è una richiesta di I/O a tutti gli effetti, il SO mette il processo nello stato`blocked`
- mentre il pezzo di processo che contiene l’indirizzo logico viene caricata in memoria principale(il SO effettua una richiesta di lettura su disco, quindi I/O a tutti gli effetti), un altro processo va in esecuzione
- quanto l’operazione I/O viene completata, un interrupt fa in modo che il processo torni `ready` (non necessariamente in esecuzione)
- quando il processo verrà eseguito, occorrerà eseguire nuovamente la stessa istruzione che aveva causato il **page fault**, questa volta però, l’indirizzo necessario è caricato nella RAM
## conseguenze
- più processi possono essere in memoria pricipale, visto che non devono essere caricati per intero
- visto che ci sono più processi in memoria principale, è più probabile che ci sia sempre un processo `ready` **aumenta il grado di multiprogrammazione**
- si possono gestire processi che richiedono più memoria di quella disponibile nella memoria principale
# memoria virtuale
uno schema che usa memoria virtuale è quindi uno schema di allorazione di memoria, in cui la memoria secondaria può essere usata come se fosse principale (nel senso che vengono tenute pagine di processi, mentre prima venivano tenuti processi per intero)
le sue caratteristiche sono:
- gli indirizzi usati nei programmi e quelli usati dal sistema sono diversi, e c’è una fase di traduzione automatica dai primi ai secondi
- la dimensione della memoria virtuale è limitata dallo schema di indirizzamento e dalla disponibilità di memoria secondaria (la RAM non influisce a riguardo)
## terminologia 
**memoria virtuale**: memoria secondaria (su disco)
**memoria reale**: memoria principale (la RAM)
**indirizzo virtuale**: l’indirizzo associato ad una locazione nella memoria virtuale, che fa sì che si possa accedere ad essa come se fosse parte della memoria principale
**spazio degli indirizzi virtuali**: la quantità di memoria virtuale assegnata ad un processo
**spazio degli indirizzi**: la quantità di memoria assegnata ad un processo
**indirizzo reale**: indirizzo di una locazione in memoria principale
## thrashing
avviene quando il SO impiega la maggior parte del suo tempo a swappare pezzi di processi, anzichè eseguire istruzioni (cioè quando quasi ogni richiesta di pagina dà luogo ad un **page fault**)
per evitare il trashing, il SO cerca di indovinare quali pezzi di processo saranno usati con maggiore probabilità nel futuro (nella prossima istruzione da eseguire), e lo fa usando un principio a noi già noto: il principio di località.
- per il principio di località, i riferimenti che fa un processo tendono ad essere vicini, quindi saranno necessari solo pochi pezzi del processo di volta in volta. in questo modo si può prevedere abbastanza bene quali pezzi di processo saranno necessari nelle istruzioni a seguire, permettendo alla memoria virtuale di evitare il thrashing e quindi di funzionare bene
# supporto hardware
paginazione e segmentazione hanno bisogno di essere supportati dell’hardware, poichè alcune operazioni sarebbero troppo lunghe se fatte in software dal SO
## traduzione degli indirizzi
### page table entry
è la tabella delle pagine: ogni processo ne ha una ed il suo PCB punta ad essa
ogni entry della tabella cont
>[!figure] traduzione degli indirizzi
>![[Pasted image 20241027172509.png]]
la traduzione è fatta dall’hardware.
precisazione sulla somma: il **page #**, prima di essere sommato al **page table Ptr**, che punta all’inizio della page table, va moltiplicato per la dimensione in byte di una entry della page table