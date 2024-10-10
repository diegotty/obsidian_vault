---
created: "2024-10-10"
related to: 
updated: "2024-10-10, 22:34"
---
//TODO index
# obiettivo
supponiamo di voler crerae una base di dati contente i seguenti dati di studenti universitari:
- dati anagrafici e identificativi: 
	- nome, cognome, data, comune e provincia di nascita, matricola, codice fiscale
- dati curriculari
	- per ogni esame sostenuto: voto, data, codice, titolo e docente del corso
# ipotesi 1
per avere tutti i dati a portata di mano, creo una sola relazione con schema: 
- Curriculum(Matr, CF, Cogn, Nome, DataN, Com, Prov, C#, Tit, Doc, DataE, Voto)
>[!figure] ![[Pasted image 20241010224236.png]]

questa relazione è piena di ridondanza: i dati necessari sono ripetuti molteplici volte nella tabella, e ciò da luogo a:
- spreco di spazio in memoria
- anomalie
## anomalie di aggiornamento
se viene aggiornato un dato, esso deve essere modificato per ogni tupla in cui si trova il dato
- se cambia un docente del corso, deve essere modificato per ogni esame sostenuto per quel corso
## anomalie di inserimento
il modo in cui è strutturata la tabella rende impossibile l’inserimento di tuple che non rispettano delle condizioni (che non dovrebbero essere necessarie)
- nell’ipotesi 1: una matricola non può essere registrata finchè non sostiene almeno un esame(dato che prendiamo come chiave matricole e esame)
## anomalie di cancellazione
eliminando i dati anagrafici di uno studente, potrebbero essere eliminati i dati di un corso (se lo studente è l’unico ad aver sostenuto l’esame di quel corso), e viceversa quando elimino un corso
# ipotesi 2
per eliminare la ridondanza, creo tre relazioni:
- Studente (Matr, Cf, Cogn, Nome, Data, Com, Prov)
- Corso (C#, Tit, Doc)
- Esame (Matr, C#, Data, Voto)
bene!! abbiamo eliminato le anomalie analizzate nell’ipotesi 1. però si possono notare delle “nuove” anomalie:
- il fatto Comune e Provincia vengano memorizzati come attributi di Studente, crea dei problemi: 
	- ridondanza: il fatto che un comune si trova in una certa provincia è ripetuto molteplici volte
	- anomalia di aggiornamento: se un comune cambia provincia, ciò va aggiornato in tutte le tuple in cui è presente il comune
	- anomalia di inserimento: non è possibile memorizzare che un comune si trova in una certa provincia se non esiste uno studente che vive in quel comune
	- anomalia di cancellazione: eleminando uno studente che vive in un determinato comune, si potrebbero perdere definitivamente le informazioni su quel comune.
# ipotesi 3
la base di dati consiste di 4 relazioni:
- Studente (Matr, Cf, Cogn, Nome, Data, Com)
- Corso (C#, Tit, Doc)
- Esame (Matr, C#, Data, Voto)
- Comune (Com, Prov)
>[!figure]  ![[Pasted image 20241010225513.png]]
>perfetto !

# principi di progettazione
uno schema di basi di dati è “buono” se non presenta
- ridondanze
- anomalie di aggiornamento, inserimento e cancellazione
spesso la cattiva progettazione, deriva da:
- rappresentare concetti distinti in un’unica relazione
- raggruppare le informazioni relative agli oggetti con le loro associazioni(al posto di recuperare dati usando riferimenti per valore tra oggetti associati, e operatori come il join, si prova a unire tutte le informazioni in un’unica tabella)
>[!warning] bisogna rappresentare separatamente ogni concetto in una relazione distinta !!

# vincoli
 ci permettono di imporre certe condizioni che sono rispettate nella realtà che si vuole rappresentare in una base di dati
 >[!example]
 >- un voto intero è compreso tra 18 e 30
 >- il numero di matricola identifica univocamente uno studente
 >- il numero di matricola in un verbale di esame deve essere il numero di matricola di uno studente
 
 un’istanza della base di dati è **legale** se soddisfa tutti i vincoli(cioè se la rappresentazione è fedele alla realtà)
## definizione di vincoli nei DBMS
un DBMS permette di 
- definire dei vincoli, insieme allo schema della base di dati
- verificare che l’istanza della base di dati sia legale
- impedire l’inserimento di tuple che violerebbero vincoli
# schema di relazione
- uno schema di relazione R è un insieme di attributi {$A_{1},\dots,A_{n}$}
- le prime lettere dell’alfabeto (A,B,C,…) denotano i singoli attributi
- le ultime lettere dell’alfabeto (X,Y,…) denotano insiemi di attributi
- se X ed Y sono insiemi di attributi XY denota $X\cup Y$
- una tupla t su R è una funzione che associa ad ogni Attributo $A_{i}$ su R un valore $t[A_{i}]$ nel corrispondente dominio dom($A_{1}$)
>[!figure] ![[Pasted image 20241010235012.png]]


Se X è un sottoinsieme di R e $t_{1}$ $t_{2}$ sono due tuple su R, $t_{1}$ e $t_{2}$ coincidono su X ($t_{1}[X]=t_{2}[X]$) se $\forall A \in X (t_{1}[A]=t_{2}[A])$