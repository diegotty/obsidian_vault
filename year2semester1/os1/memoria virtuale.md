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
- mentre la pagina di processo che contiene l’indirizzo logico viene caricata in memoria principale(il SO effettua una richiesta di lettura su disco, quindi I/O a tutti gli effetti), un altro processo va in esecuzione
- quanto l’operazione I/O viene completata, un interrupt fa in modo che il processo torni `ready` (non necessariamente in esecuzione)
- quando il processo verrà eseguito, occorrerà eseguire nuovamente la stessa istruzione che aveva causato il **page fault**, questa volta però, l’indirizzo necessario è caricato nella RAM
## conseguenze
- più processi possono essere in memoria pricipale, visto che non devono essere caricati per intero
- visto che ci sono più processi in memoria principale, è più probabile che ci sia sempre un p