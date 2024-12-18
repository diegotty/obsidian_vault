---
created: 2024-12-18
related to: 
updated: 2024-12-18T13:13
---
l’area di memoria di un processo caricare in memoria (principale) è diviso nelle sezioni seguenti:
>[!figure] area di memoria di un processo
![[Pasted image 20241218131014.png]]

lo stack è costituito da stack frames, e ciascun frame contiene:
- parametri passati alla funzione
- variabili locli
- indirizzo di ritorno
- instruction pointer

- lo stack pointer punta alla cima dello stack 
- il frame pointer punta alla base del frame corrente