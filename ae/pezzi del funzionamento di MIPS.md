
## organizzazione della memoria
![[Pasted image 20240317141855.png]]

| name                | function                                                                                                                                       |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| SP(stack pointer)   | indica fino a dove interpretare lo spazio libero come memoria in cui sono allocate le chiamate nidificate e le variabili locali delle funzioni |
| GP(global pointer)  | indica fino a dove intrepretare lo spazio libero come memoria in cui sono allocati i dati dinamici non-locali                                  |
| PC(program counter) | tiene traccia dell’istruzione corrente(dove ci troviamo). per passare all’istruzione successiva va incrementato di 4 byte !                    |
## fasi di esecuzione
- fetch/caricamento dell’istruzione(dalla posizione indicata dal program counter)
- decodifica(lettura dell’op code)
- load (caricamento di eventuali argomenti)
- esecuzione
- store del risultato(in memoria o registro)
- aggiornamento del program counter
## modi di indirizzamento
![[Pasted image 20240317143059.png]]
![[Pasted image 20240317143003.png|400]]
![[Pasted image 20240317143111.png]]
//kinda interesting but boring ngl