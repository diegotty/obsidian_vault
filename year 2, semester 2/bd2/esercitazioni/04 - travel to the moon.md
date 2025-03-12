---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-12T16:40
completed: false
---
# raffinazione
1. crociere
	1. codice (stringa)
	2. data di inizio
	3. data di fine
	4. nave utilizzata (vedi req. 2)
	5. itinerario seguito (vedi req. 4)
2. navi
	1. nome
	2. gardo di comfort(un intero tra 3 e 5)
	3. capienza (intero > 0)
3. destinazioni
	1. nome
	2. continente (tipo concettuale tra i 5 continenti)
	3. posti da vedere (vedi req. 5)
	4. tipo (uno tra romantica e divertente)
	5. se è esotica (se si trova in un continente diverso dall’europa)
4. itinerari
	1. nome
	2. sequenza ordinata di elementi, di cui interessa 
		1. destinazione
		2. arrivo
			1. ora
			2. numero d’ordine del giorno (rispetto alla data di inizio della crociera)
		3. ripartenza
			1. ora
			2. numero d’ordine del giorno(rispetto alla data d’inizio della crociera)
5. posti da vedere
	1. nome
	2. fascia oraria