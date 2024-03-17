
## organizzazione della memoria
![[Pasted image 20240317141855.png]]

| name                | function                                                                                                                                       |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| SP(stack pointer)   | indica fino a dove interpretare lo spazio libero come memoria in cui sono allocate le chiamate nidificate e le variabili locali delle funzioni |
| GP(global pointer)  | indica fino a dove intrepretare lo spazio libero come memoria in cui sono allocati i dati dinamici non-locali                                  |
| PC(program counter) | tiene traccia dell’istruzione corrente(dove ci troviamo). per passare all’istruzione successiva va incrementato di 4 byte !                    |
## fasi di esecuzione
- fetch/caricamento dell’istruzione(dalla posizione indicata dal prog)
- decodifica
- load
- esecuzione
- store del risultato(in memoria o registro)
- aggiornamento del program counter