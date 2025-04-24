---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-24T17:44
completed: false
---
programmazione dinamica → tabella
di solito, per la programmazione dinamica si usa l’approccio bottom-up

x fib bottom-up possiamo sbarazzarci della tabella

a,b = b, a+b (spazio $O(1)$)

il conteggio delle stringhe di lunghezza n senza 2 zeri di file è fibonacci



alberghi
abbimo n persone da distribuire in un albergo con stanze singole e doppie. in quanti modi si possono distribuire le persone ?

t[i] = numero di modi in cui posso sistemare i persone
quando aggiungo una persona, o la metto in una stanza singola, o in coppia con una persona

se lo metto in coppia con una persona, può essere uno tra gli i-1 persone rimanenti. una volta scelta la persona col compagno, comb
t[i] = t[i-1] + (i-1) cdot t[i-2]