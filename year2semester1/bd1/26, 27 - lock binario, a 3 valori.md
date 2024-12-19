---
created: 2024-12-19
related to: 
updated: 2024-12-19T11:18
---
# lock
**lock**: privilegio di accesso ad un singolo item, realizzato mediante una variabile associata all’item (**variabile lucchetto**), il cui valore descrive lo stato dell’item rispetto alle operazioni che possono essere effettuate su di esso
- un lock viene richiesto da una transazione mediante un’operazione di **locking** (`lock(X)`): se il valore della variabile è `unlocked`, la transazione può accedere all’item e alla variabile viene assegnato il valore `locked`
- un lock viene rilasciato da una transazione mediante un’operazione di **unlocking** (`unlock(X)`), che assegna alla variabile il valore `unlocked`
- fra l’esecuzione di un’operazione di locking su un dato item $X$ e l’operazione di unlocking su tale $X$, la transazione **mantiene un lock** su $X$
il locking agisce quindi come primitiva di sincronizzazione !
## schedule legale
uno schedule è detto **legale** se una transazione effettua un locking ogni volta che deve leggere o scrivere un item, e ciascuan transazione rilascia ogni lock che ha ottenuto
## lock binario
un lock binario può assumere solo 2 valori: `locked` e `unlocked`
le transazioni fanno uso di due operazioni: 
- `lock(X)` per chiedere l’accesso all’item $X$
- `unlock(X)` per rilasciare l’item $X$ consentendone l’accesso ad altre transazioni

consideriamo di nuovo l’esempio introdotto in [[25 - controllo della concorrenza#problemi]]
>[!example] dati con lock
riscriviamo le transazioni utilizzando le primitive di lock binario
![[Pasted image 20241219105813.png]]

>[!example] esempio 1: ghost update
>questo schedule, che replica lo schedule dell’esempio 1 senza lock binario, è legale, e risolve il problema dell’aggiornamento perso
![[Pasted image 20241219105847.png]]

### equivalenza
vedremo che la proprietà di equivalenza degli schedule dipende dal protocollo di locking adottato
nel caso semplice del lock binario a 2 valori, formalizziamo il concetto di equivalenza adottando un modello delle transazioni che astrae dalle specifiche operazioni e si basa su quelle rilevanti per valutare le sequenze degli accessi, cioè in questo caso, `lock()` e `unlock()`
>[!info] modello per transazioni
>![[Pasted image 20241219110236.png]]
>in questo modello, una transazione è quindi una sequenza di operazioni di `lock()` e `unlock()`, in cui:
>- ogni `lock(X)` implica la lettura di $X$
>- ogni `unlock(X)` implica la scrittura di $X$

in corrispondenza di una scrittura, viene associato un nuovo valore all’item coinvolto, che viene calcolato come una funzione che:
- è associata in modo univoco ad ogni coppia di `lock-unlock`
- ha per argomenti **tutti** gli item **letti**(`locked`) dalla transazione prima dell’operazione di `unlock` (perchè magari i loro valori hanno contribuito all’aggiornamento dell’item corrente)
date queste nozioni:
>[!important] equivalenza
due schedule sono equivalenti se le formule che danno i valori finali per ciascun (e tutti) item sono le stesse 

>[!important] serializzabilità
uno schedule è **serializzabile** se è equivalente ad uno schedule seriale (basta trovarne uno)

>[!example] esempio 
consideriamo le transizioni:
![[Pasted image 20241219111131.png]]
e lo schedule con interleaving:
![[Pasted image 20241219111201.png]]
in questo caso il valore finale di $X$ è $f_{4}(f_{1}(X_{0}), Y_{0})$ e il valore finale di $Y$ è $f_{2}(X_{0}, f_{3}(Y_{0}))$
>
>calcoliamo ora gli schedule sequenziali, per vedere se lo scheduling sopra è serializzabile:
![[Pasted image 20241219111414.png]]
>in questo caso, il valore finale di $X$ è $f_{4}(f_{1}(X_{0}),f_{2}(X_{0},Y_{0}))$, e il valore finale di $Y$ è $f_{3}(f_{2}(X_{0},Y_{0})$
>
>![[Pasted image 20241219111727.png]]
>in questo caso, il valore finale di $X$ è $f_{1}(f_{4}(X_{0},Y_{0}))$, e il valore finale di $Y$ è $f_{2}(f_{4})$
