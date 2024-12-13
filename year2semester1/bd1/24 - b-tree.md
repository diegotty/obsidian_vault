---
created: 2024-12-13
related to: 
updated: 2024-12-13T11:46
---
# b-tree
anche in questo caso, si lavora su dati ordinabili per chiave univoca.
il b-tree nasce dalla generalizzazione della struttura di indice, creando più di livelli del file indice, in modo da poter accedere al file principale attraverso una gerarchia di indici. l’indice a livello più alto nella gerarchia, la **radice**, è **costituito da un unico blocco**, e quindi può risiedere in memoria principale durante l’utilizzo del file.

ogni blocco di un file indice è costituito di record contenenti una coppia $(v,b)$ dove $v$ è il valore della chiave del primo record della porzione del file principale che è accessibile attraverso il puntatore $b$
- $b$ può essere un puntatore ad un blocco del file indice a livello immediatamente più basso, oppure ad un blocco nel file principale
- il primo record indice di ogni blocco indice contiene solo un puntatore ad un blocc