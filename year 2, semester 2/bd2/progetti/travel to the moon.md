---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-29T16:56
completed: false
---
## raffinamento dei requisiti
1. Requisiti sulle crociere:
	1. codice 
	2. data di inizio
	3. data di fine
	4. nave utilizzata (v. req. 2)
	5. itinerario (v. req. 4)
	6. il tipo, uno tra:
		1. luna di miele, di cui interessa:
			1. sottotipo, uno tra:
				1. tradizionali: 
					quelle che prevedono un numero di destinazioni romantiche (v. req. 3.4) maggiore o uguale al numero di destinazioni divertenti
				2. alternative
					quelle non tradizionali
		2. per famiglie, di cui interessa:
			1. se adatte ai bambini (booleano)

2. Requisiti sulle navi
	1. nome
	2. comfort (3..5)
	3. capienza

3. Requisiti sulle destinazioni:
	1. nome
	2. continente
	3. posti da vedere (v. req. 5)
	4. tipo, almeno uno tra:
		1. romantico
		2. divertente
	5. se è esotica, ovvero se è fuori dall'Europa

4. Requisiti sugli itinerari: di ogni itinerario interessa
	1. sequenza ordinata di elementi, di cui interessa:
		1. destinazione (v. req. 3)
		2. arrivo:
			1. il numero d'ordine del giorno (rispetto alla data di inizio della crociera)
			2. ora
		3. ripartenza	
			1. il numero d'ordine del giorno (rispetto alla data di inizio della crociera)
			2. ora

5. Requisiti sui posti da vedere:
	1. nome
	2. descrizione
	3. orari di apertura, nella forma di una mappa che associa ad ogni giorno della settimana (lunedì, ..., domenica) un insieme di fasce orarie, dove ogni fascia oraria è definita in termini di una coppia di orari

6. Requisiti sui clienti:
	1. nome
	2. cognome 
	3. età 
	4. indirizzo

7. Requisiti sulle prenotazioni di crociere da parte dei clienti:
	1. cliente
	2. crociera
	3. istante di prenotazione
	4. numero di posti prenotati

8. Requisiti sulle funzionalità
	1. Accedute dal personale dell'Ufficio Prenotazioni:
		1. Effettuare la prenotazione, per conto di un cliente, di un certo numero di posti per una crociera (non ancora partita), solo se la nave ha ancora un numero di posti disponibili sufficiente
	2. Accedute dall'Ufficio Marketing:
		1. Dato un periodo 'p', calcolare l'età media dei clienti che hanno prenotato, durante 'p', almeno una crociera che prevede una destinazione esotica (v. req. 3.5)
		2. Dato un periodo 'p', calcolare la percentuale delle destinazioni 'gettonate' in 'p', ovvero raggiunte, durante 'p', da almeno dieci crociere di luna di miele, oppure da almeno quindici crociere per famiglie.
## diagramma UML
![[Pasted image 20250324182923.png]]
## specifica dei tipi
- Ora: (ore: Intero in 0..24, minuti: Intero in 0..60)
- Indirizzo: (via : Stringa, civico : Intero > 0, cap : Intero > 0)
- Orario: (apertura : Ora, chiusura : Ora)
- Giorno : {lunedì, martedì, mercoledì, giovedì, venerdì, sabato, domenica}
### modifiche
- `fine()`: la data di fine della crociera è calcolabile(calcolabile → operazione)
- `posti_disponibili(t: DataOra): Intero >= 0`: modelliamo un’operazione, in modo da sapere il numero di posti disponibili in un istante di tempo
- tipo di luna di miele: `tipo(): {trad, alt}`, perchè bisogna contare le tappe (è quindi un calcolo → operazione)
- `durata_g()`: durata in giorni del'l’itinerario
- l’ordine delle tappe si gestisce da solo, in quanto sappiamo in che giorno (giorno relativo all’inizio) si arriva e si riparte da ogni tappa
- `Continente` viene modellato come classe in quanto, in questo caso ci interessano i continenti “a se stanti” (magari per qualche interrogazione), e se non ci fosse una destinazione in uno dei continenti, tale continente non sarebbe presente (e ciò sarebbe errato)
- `estotica()`: avendo il continente, possiamo calcolare se una destinazione è esotica (quindi non è un attributo, ma un’operazione)
- `partenza_iniziale` e `arrivo_finale` vengono usate per specificare in modo diverso da delle tappe intermedie la partenza e l’arrivo. vengono usate association class per aggiungere attributi rilevanti 
- `TipoDestinazione`: modellamo la classe e instanzieremo poi a livello estensionale i due tipi (divertente, romantica)