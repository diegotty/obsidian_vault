---
created: 2024-12-19
related to: 
updated: 2024-12-19T10:48
---
**lock**: privilegio di accesso ad un singolo item, realizzato mediante una variabile associata all’item (**variabile lucchetto**), il cui valore descrive lo stato dell’item rispetto alle operazioni che possono essere effettuate su di esso
- un lock viene richiesto da una transazione mediante un’operazione di **locking** (`lock(X)`): se il valore della variabile è `unlocked`, la transazione può accedere all’item e alla variabile viene assegnato il valore `locked`
- un lock viene rilasciato da una transazione mediante un’operazione di **unlocking** (`unlock(X)`), che assegna alla variabile il valore `unlocked`
- fra l’esecuzione di un’operazione di locking su un item e l’operazione di unlocking su tale $X$, la transazione **mantiene un lock** su $X$
il locking agisce quindi come primitiva di sincronizzazione !
