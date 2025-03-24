---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-24T18:01
completed: false
---
```
1. Requisiti sulle crociere:
	1.1. codice 
	1.2. data di inizio
	1.3. data di fine
	1.4. nave utilizzata (v. req. 2)
	1.5. itinerario (v. req. 4)
	1.6. il tipo, uno tra:
		1.6.1. luna di miele, di cui interessa:
			1.6.1.1. sottotipo, uno tra:
				1.6.1.1.1. tradizionali: 
					quelle che prevedono un numero di destinazioni romantiche (v. req. 3.4) maggiore o uguale al numero di destinazioni divertenti
				1.6.1.1.2. alternative
					quelle non tradizionali
		1.6.2. per famiglie, di cui interessa:
			1.6.3. se adatte ai bambini (booleano)


2. Requisiti sulle navi
	2.1. nome
	2.2. comfort (3..5)
	2.3. capienza



3. Requisiti sulle destinazioni:
	3.1. nome
	3.2. continente
	3.3. posti da vedere (v. req. 5)
	3.4. tipo, almeno uno tra:
		3.4.1. romantico
		3.4.2. divertente
	3.5. se è esotica, ovvero se è fuori dall'Europa


4. Requisiti sugli itinerari: di ogni itinerario interessa
	4.1. sequenza ordinata di elementi, di cui interessa:
		4.1.1. destinazione (v. req. 3)
		4.1.2. arrivo:
			4.1.2.1. il numero d'ordine del giorno (rispetto alla data di inizio della crociera)
			4.1.2.2. ora

		4.1.3. ripartenza	
			4.1.3.1. il numero d'ordine del giorno (rispetto alla data di inizio della crociera)
			4.1.3.2. ora


5. Requisiti sui posti da vedere:
	5.1. nome
	5.2. descrizione
	5.3. orari di apertura, nella forma di una mappa che associa ad ogni giorno della settimana (lunedì, ..., domenica) un insieme di fasce orarie, dove ogni fascia oraria è definita in termini di una coppia di orari

6. Requisiti sui clienti:
	6.1. nome
	6.2. cognome 
	6.3. età 
	6.4. indirizzo

7. Requisiti sulle prenotazioni di crociere da parte dei clienti:
	7.1. cliente
	7.2. crociera
	7.3. istante di prenotazione
	7.4. numero di posti prenotati



8. Requisiti sulle funzionalità
	8.1. Accedute dal personale dell'Ufficio Prenotazioni:
		8.1.1. Effettuare la prenotazione, per conto di un cliente, di un certo numero di posti per una crociera (non ancora partita), solo se la nave ha ancora un numero di posti disponibili sufficiente

	8.2. Accedute dall'Ufficio Marketing:
		8.2.1. Dato un periodo 'p', calcolare l'età media dei clienti che hanno prenotato, durante 'p', almeno una crociera che prevede una destinazione esotica (v. req. 3.5)

		8.2.2. Dato un periodo 'p', calcolare la percentuale delle destinazioni 'gettonate' in 'p', ovvero raggiunte, durante 'p', da almeno dieci crociere di luna di miele, oppure da almeno quindici crociere per famiglie.
```
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
	2. orari di apertura (una mappa che associa ad ogni giorno della settimana ad un insieme di  fasce oraria, definita in termini di una coppia di orari)
## specifica dei tipi
- Periodo: (data_inizio: Data, data_fine: Data)
- Continente: {A}