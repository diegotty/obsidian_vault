---
created: "2024-10-24"
related to: 
updated: "2024-10-24, 20:04"
---
//TODO index
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
>[!info] precisazioni (IMPORTANTI)
>- una chiave è una superchiave
>- un attributo $A$ è primo se esiste una chiave $K$ che contiene $A$
>- nella definizione, è importante ricordarsi che $A \notin X$ perchè $A$ è un **singleton**: è un elemento singolo, non un sottoinsieme di $X$, altrimenti ci sarebbe scritto $A \not\subset X$
>- dato che $A$ è un singleton, le dipendenze $X \to A$ le “prendiamo” da $F^+$, poichè siamo sicuri, che grazie alla regola di decomposizione, troveremo tutte le dipendenze di $F$ con dipendenti non singleton in $F^+$, decomposte nella forma $X \to A$
>- inoltre, con $A \notin X$ sto eliminando tutte le dipendenze funzionali banali (che sono le dipendenze ottenute applicando l’assioma di Armstrong della riflessività), in quanto se controllassimo anche le dipendenze banali nessuna tabella sarebbe in 3FN (per i campi che non sono chiave)
>- per quanto detto, è sbagliato scrivere $\forall X \to A \in F$, perchè non sapremmo se e come valutare una dipendenza del tipo $X \to AB$ (due o più attributi a destra), e se sostituisco $\forall X \to A \in F$ con $\forall X \to Y \in F$, (quindi accetto dipendenti che non sono singleton) non so come comportarmi se $Y$ contiene sia attributi primi che non primi
>- basta identificare anche UNA SOLA dipendenza che viola le condizione per la 3FN affinchè l’intera relazione non sia in 3FN
>-

>[!info] important stuff about 3FN
> in terza forma normale si può sempre trovare uno schema in cui si rispettano le dipendenze originarie (decomponendo la relazione originale), anche se gli attributi usati nelle dipendenze vengono distribuiti in tabelle diverse

>[!example] esempio alla lavagna
$$R = ABCD$$
$$F = \{AB \to CD, AC \to BD, D \to BC\}$$
>AB è chiave, AC è chiave, AD è chiave (applico aumento sull’ultima dipendenza)
>se guardo F senza calcolare le chiusure, la dipendenza $D \to BC$ ha come determinante $D$ che non è una chiave(MA è parte di una chiave !!!!)
>- inoltre il dipendente è un attributo primo (decomponendo la dipendenza e anlizzandone le versioni decomposte, vediamo che B appartiene ad una chiave, C appartiene ad una chiave)
>quindi la dipendenza $D \to BC$  non viola la 3FN

>[!example] esempio 3
$$R = ABCD$$
$$F = \{AB \to CD, BC \to A, D \to AC\}$$
abbiamo tre chiavi: $AB, BC, DB$ infatti:
$AB$ per dipendenza $AB \to CD$, $DB$ per aumento su $B$ per la dipendenza $D \to AC$, e $BC$ per aumento su $B$ per la dipendenza $BC \to A$ su cui poi si può usare la transitività con la dipendenza $AB \to CD$
ora controlliamo se R è in 3FN:
>- $AB \to CD$ è ok ($AB$ è superchiave)
>- $BC \to A$ è ok ($A$ è primo, $BC$ è superchiave)
>- $D \to AC$ è ok ($AC$ è primo(possiamo analizzare le dipendenze derivate dalla decomposizione, $A$ è primo e $C$  è primo))
>quindi la relazione è in 3FN !!!
## definizione alternativa
dato uno schema R e un insieme di dipendenze funzionali F, R è in 3FN se e solo se non ci sono attributi che dipendono parzialmente o transitivamente da una chiave
# dipendenza parziale
$X \to A \in F^+, A \notin X$ è una dipendenza parziale su R se $A$ non è primo ed $X$ è contenuto propriamente in una chiave di R.
>[!figure] ![[Pasted image 20241024221729.png|200]]
$X$ proriamente contenuto in una chiave $K$ di R

# dipendenza transitiva
$X \to A \in F^+, A \notin X$ è una dipendenza transitiva su R se $A$ non è primo e per ogni chiave $K$ di R si ha che $X$ **non** è contenuto propriamente in $K$ e $K-X \neq \varnothing$
>[!figure] ![[Pasted image 20241024222117.png|400]]
