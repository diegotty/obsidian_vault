---
created: 2025-01-05T12:53
updated: 2025-01-05T13:58
---
## esercizio 1
theta join: dell’attributo su cui si fa il theta join rimane solo una copia, quella di sinistra (?, per il noome è importante)

## esercizio 2
chiavi dello schema : [[12 - identificazione delle chiavi di uno schema]]

>[!info] osservazioni
>1. conviene partire dai sottoinisiemi di $ABH$ di cardinalità maggiore: se la loro chiusura non contiene $R$, è inutila calcolare la chiusura dei loro sottoinsiemi
>2. gli attributi che **non compaiono mai a destra delle dipendenze funzionali di F**, non sono determinati funzionalmente da nessun attributo, quindi **dovranno essere sicuramente in ogni chiave** (perchè rimarebbero fuori da qualunque chiusura transitiva, e una chiave deve determinare l’intero schema)
>3. gli attributi che compaiono sempre e solo come dipendenti non potranno stare in una chiave
>4. gli attributi che non compaiono mai( nè determinante, nè dipendente) nelle dipendenze funzionali in $F$, devono stare per forza nella chiave