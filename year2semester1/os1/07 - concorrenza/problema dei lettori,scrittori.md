---
created: 2024-12-09
related to: 
updated: 2024-12-09T20:40
---
a differenza del problema [[intro alla concorrenza#problema del produttore/consumatore|problema del producer/consumer]], le condizioni da soddisfare sono le seguenti:
- più lettori possono leggere il buffer contemporaneamente
- un solo scrittore può leggere il buffer
- se uno scrittore è all’opera sul buffer, nessun lettore può effettuare letture
inoltre, a differenza del problema producer/consumer, il buffer si accede **per intero**
- il buffer è quindi un’area condivisa **intera**, e non ci sono problemi di buffer pieno/vuoto