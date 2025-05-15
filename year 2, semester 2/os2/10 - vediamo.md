---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-15T09:01
completed: false
---
## flussi di esecuzione concorrente
come abbiamo studiato, la fonte di complicazione maggiore nei SO è costituita dall’esistenza di flussi di esecuzione concorrenti, in quanto:
- la coerenza delle strutture dati private di ciascun flusso di esecuzione **è garantita** dal meccanismo di cambio del flusso
- la coerenza delle strutture dati condivise tra i vari flussi di esecuzione **non è garantita** da tale meccanismo !
abbiamo già visto che se due o più flussi di esecuzione hanno una struttura dati in comune, la concorrenza dei flussi può determinare uno stato della struttura **non coerente** con la logica di ciascuno dei flussi ! 
- possono infatti accadere **race condition**, situazioni in cui lo stato della memoria condivisa tra due o + flussi di esecuzione concorrenti **dipende dall’ordine esatto degli accessi in memoria** (grave !)