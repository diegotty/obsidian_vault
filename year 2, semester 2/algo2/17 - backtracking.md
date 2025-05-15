---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-15T11:46
completed: false
---
abbiamo fatto:
es1
es2
es3 ?
esame ottobre 2020
esame settembre 2020
>[!example] problema
progettare un algoritmo che prende come parametro un intero $n$ e **stampa** tutte le stringhe binarie lunghe $n$

>[!info] soluzione
notiamo che le stringhe da stampare sono $2^n$, e che stampare una stringha lunga $n$ costa $\Theta(n)$ (se immaginiamo la stringa come una lista a cui aggiungere)
quindi, per l’algoritmo che risolve questo problema, abbiamo $\Omega(n\cdot 2^n)$

implementando una soluzione 