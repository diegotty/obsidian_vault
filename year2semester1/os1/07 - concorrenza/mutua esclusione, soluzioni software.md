---
created: 2024-12-09
related to: 
updated: 2024-12-09T10:29
---
proviamo ora a gestire la mutua esclusione senza aiuto dal parte dell’hardware o dal SO. gestiremo quindi tutto nel codice (senza la sicurezza di avere operazioni atomiche).
>[!important] le soluzioni che vedremo valgono per 2 processi
>fare il passaggio a $n$ processi è possibile ma non semplice

>[!info]