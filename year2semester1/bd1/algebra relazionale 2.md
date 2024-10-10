---
created: "2024-10-10"
related to: 
updated: "2024-10-10, 07:29"
---
//TODO index 
fino ad ora abbiamo visto query che implicavano condizioni equivalenti al quantificatore universale:
$$\exists \text{(esiste almeno un)}$$
ciò ci è comodo perchè:
>[!info] in qualunque posizione appaiano nell’espressione di algebra relazionale, la valutazione delle condizioni avviene in sequenza, tupla per tupla, e quando si incontra una tupla che soddisfa le condizioni, viene inserita nel risultato

>[!example] query: Codici dei clienti che hanno effettuato un ordine per più di 100 pezzi
>![[Pasted image 20241010073654.png]]
>(se poi non ci interessano i duplicati, possiamo proiettare su C#)

alcune query, però, potrebbero richiedere la valutazione di gruppi interi di tuple, prima di decidere se inserirle tutte, qualcuna o nessuna nel risultato. però:
- le tuple non sono ordinate
- la valutazione, come definito prima, avviene in sequenza, tupla per tupla, e una volta inserita una tupla nel risultato, non possiamo più eliminarla
in questo caso, la condizione equivale a valutare il quantificatore universale 
$$\forall \text{ (per ogni)} \text{ o } \not\exists \text{ (non esiste nessun)}$$