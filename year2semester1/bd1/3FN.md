---
created: "2024-10-24"
related to: 
updated: "2024-10-24, 20:04"
---
ritorniamo all’esempio in [[progettazione, problemi e vincoli]], e ripartiamo dalla soluzione “buona” trovata alla fine
# ipotesi 3
>[!info] example
la base di dati consiste di 4 relazioni:
>- $Studente (Matr, Cf, Cogn, Nome, Data, Com)$
>- $Corso (C\#, Tit, Doc)$
>- $Esame (Matr, C\#, Data, Voto)$
>- $Comune (Com, Prov)$

consideriamo singolarmente le tabella della base di dati, e troviamo l’insieme delle dipendenze funzionali
## Studente
poichè il numero di matricola identifica univocamente uno studente, ad ogni numero di matricola corrisponde:
- un solo codice fiscale $(Matr \to CF)$
- un solo cognome $(Matr \to Cogn)$
- un solo nome $(Matr \to Nome)$
- una sola data di nascita ($Matr \to Data$)
- un solo comune di nascita ($Matr \to Com$)
quindi, usando la regola dell’unione:
$$Matr \to Matr,CF,Cogn,Nome,Data ,Com$$
con considerazioni analoghe abbiamo che un’istanza di studente, per essere legale, deve soddisfare la dipendenza funzionale:
$$CF \to Matr, CF, Cogn, Nome, Data,Com$$
pertanto sia $Matr$ che $CF$ sono chiavi di Studente
>[!warning] in questo caso ci è facile iniziare direttamente da una chiave, ma negli esercizi può avere senso iniziare da una superchiave per poi “restringere” i determinanti fino a trovare una chiave

d’altra parte, possiamo osservare che ci possono essere 2 studenti con lo stesso cognome e nomi differenti, quindi la dipendenza funzionale $Cogn \to Nome$ non è necessariamente soddisfatta
- con considerazioni analoghe, possiamo concludere che le uniche dipendenze funzionali che devono essere soddisfatte da un’istanza legale sono del tipo:
$$K \to X$$
dove $K$ contiene una chiave ($Matr$ o $CF$)
>[!info] questa è una prima condizione che però va ulteriormente rifinita per arrivare ad una definizione precisa di 3FN
## Esame
uno studente può sostenere l’esame relativo ad un corso una sola volta: quindi, per ogni esame esiste:
- una sola data
- un solo voto
quindi ogni istanza legale di Esame deve soddisfare la dipendenza funzionale:
$$Matr, C\# \to Data, Voto$$
pertanto, $Matr, C\#$ è una chiave per Esame, e anche l’unica, perchè:
$Matr$ non può fare da determinante nè per $Data$, nè per $Voto$ (intuitivo), e la stessa considerazione si può fare con $C\#$ come determinante
## Corso, Comune
con lo stesso ragionamento, arriviamo a determinare le chiavi per tutti gli schemi della base di dati:
>[!example] risultato
>- $Studente (\textbf{Matr}, Cf, Cogn, Nome, Data, Com)$
>- $Corso (\textbf{C\#}, Tit, Doc)$
>- $Esame (\textbf{Matr, C\#}, Data, Voto)$
>- $Comune (\textbf{Com}, Prov)$

>[!warning]
stiamo continuando ad assumere che $Com \to Prov$, cioè che non ci sono comuni ononimi (cosa che in realtà non è vera)

# 3FN
dati uno schema di relazione R e un insieme di dipendenze funzionali F su R, R è in 3FN se:
$$\forall X \to A \in F^+, A \notin X$$
in cui:
- $A$ **è primo**(appartiene ad una chiave)
**oppure**
- $X$ **è superchiave**(contiene una chiave)
>[!info] precisazioni
- una chiave è una superchiave
- un attributo $A$ è primo se esiste una chiave $K che contiene A