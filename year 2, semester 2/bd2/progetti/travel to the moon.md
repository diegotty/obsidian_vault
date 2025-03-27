---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-27T12:34
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
- Periodo: (data_inizio: Data, data_fine: Data)
- Continente: {Africa, Asia, America, Europa, Oceania}
- TipoDestinazione: {divertente, romantica}
- DataTappa(giorno: Intero > 0, ora: Orario) 
- Orario: (ore: Intero in 0..24, minuti: Intero in 0..60)
- TipoLunaDiMiele: {tradizionale, alternativa}
### modifiche
- dataFine non è un dato. è calcolabile. usiamo quinid un’operazione `fine()`
- in comfort non è necessario mettere intero, in quanto i range sono usati solo con interi
- `posti_disponibili(t: DataOra): Intero >= 0`: modelliamo un’operazione
attributi opzionali potrebbero hintare a generalizzazioni non fatte ! e sempre meglio modellare la struttura, senza perderla in ragionamenti impliciti (es: crocieraperfamiglie)
- tipo di luna di miele: `tipo(): {trad, alt}`, perchè devi contare le tappe,
- estotica: avendo il continente, ce lo possiamo calcolare (non è un attributo, ma un’operazione)
- bisogna creare classe continente, in quanto in questo caso ci interessano i continenti a se stanti, e se non ci fosse una destinazione in continente x, il continente non “esisterebbe” nella base di dati, che è sbagliato
- tipo destinazione: è una classe, con cui credo un’associazione con destinazione
- per l’ordine delle tappe, gestissco attraverso 
- un itinerario deve avere arrivo e partenza(quindi uso 2 associazioni, destinazione_arrivo e dest_partenza)
	- partenza ha association class con attributo ora (utile saperlo)
	- la data dell’arivvo la ricaviamo dalla durata della crociera (`fine()` ?)

differenza tra use case e operazione: use-case modifica il livello estensionale, mentre operazione no