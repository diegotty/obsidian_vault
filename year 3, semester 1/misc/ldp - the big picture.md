---
related to:
created: 2025-03-02T17:41
updated: 2025-10-25T20:19
completed: false
---
il corso studia la semantica dei linguaggi di programmazione
siamo partiti dalla definizione di Peano dei *numeri naturali*, e abbiamo notato come essa sia un caso speciale della definizione di *algebra*
per le algebre, abbiamo studiato la *chiusura* di un insieme rispetto ad un’operazione (anche per *algebre eterogenee*)
abbiamo definito le *algebre induttive*, e abbiamo studiato le algebre induttive con la stessa segnatura, per definire gli *omomorfismi*, gli *isomorfismi*, e il *teorema di Lambek*

abbiamo introdotto le *espressioni* (in *backus-naur form*), le abbiamo definite come algebre e ci siamo soffermati sul linguaggio $EXP$.
- del linguaggio $EXP$, abbiamo abbiamo definito le *variabili libere* e le *variabili legate* di un’espressione, e il loro *scope*
abbiamo introdotto la valutazione dei termini del linguaggio $EXP$, e per farlo sono stati definiti gli *ambienti*. grazie agli ambienti, abbimo definito la *semantica operazionale* (il significato dei termini di $EXP$), attraverso delle *regole di inferenza*

durante l’applicazione delle regole di inferenza, abbiamo notato come per determinati termini, sarebbe più veloce applicare un metodo diverso per valutare i tali.
sono stati quindi introdotti gli *approcci* alla valutazione:
- *approccio eager*
- *approccio lazy* 
e le varianti di entrambi: *statico*  *dinamico*