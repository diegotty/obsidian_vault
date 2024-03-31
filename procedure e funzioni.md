frammento di codice che riceve degli argomenti e calcola un risultato (utile per rendere il codice riusabile e modulare !)
- ha un indirizzo di partenza
- riceve uno o o più argomenti
- svolge un calcolo
- ritorna un risultato
- continua la sua esecuzione dall’istruzione seguente a quella che l’ha chiamata ( a livello di program counter)

>[!tuff] usiamo jal(jump and link) to “call” functions

# istruzioni necessarie

## jal
ci permette di saltare ad un indirizzo, ma salva in `$ra` l’address della prossima istruzione **del chiamante**, quindi a fine funzione basta saltare all’indirizzo in `$ra` (usando `jr`) per tornare alla riga giusta del chiamante

