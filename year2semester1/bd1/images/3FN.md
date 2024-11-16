---
created: 2024-10-24
related to: "[[progettazione, problemi e vincoli]]"
updated: 2024-10-24, 20:04
---
>[!index]
>
>- [ipotesi 3](#ipotesi%203)
>	- [Studente](#Studente)
>	- [Esame](#Esame)
>	- [Corso, Comune](#Corso,%20Comune)
>- [3FN](#3FN)
>- [dipendenza parziale](#dipendenza%20parziale)
>- [dipendenza transitiva](#dipendenza%20transitiva)
>- [definizione alternativa di 3FN](#definizione%20alternativa%20di%203FN)
>	- [dimostrazione](#dimostrazione)
>		- [schema in 3FN $\implies$ no dipendenze parziali o transitive](#schema%20in%203FN%20$%5Cimplies$%20no%20dipendenze%20parziali%20o%20transitive)
>		- [no dipendenze parziali o transitive$\implies$schema in 3FN](#no%20dipendenze%20parziali%20o%20transitive$%5Cimplies$schema%20in%203FN)
>- [decomposizione](#decomposizione)
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
# dipendenza parziale
$X \to A \in F^+, A \notin X$ è una dipendenza parziale su R se $A$ non è primo ed $X$ è contenuto propriamente in una chiave di R.
>[!figure] ![[Pasted image 20241026181728.png]]
$X$ proriamente contenuto in una chiave $K$ di R

>[!example]
$\text{Curriculum(\textbf{Matr}, CF, Cogn, Nome, DataN, Com, Prov, \textbf{C\#}, Tit, Doc, DataE, Voto)}$
ad un numero di matricola corrisponde un solo cognome, il cognome dello studente con quella matricola, quindi: 
$$Matr \to Cogn$$
quindi, ad una coppia di matricola e codice fiscale uguali, corrisponderà un solo cognome:
$$Matr, C\# \to Cogn$$
**l’attributo Cogn però, dipende parzialmente dalla chiave $\textbf{Matr, C\#}$**, perchè la dipendenza $Matr, C\# \to Cognome$ è una conseguenza di $Matr \to Cogn$.
la dipendenza $Matr \to Cogn$ è quindi detta dipendenza parziale
(in soldoni: è una dipendenza in cui la determinante è parte di una chiave (non una chiave) e il dipendente  non è primo)

dal punto di vista dell’attributo che dipende parzialmente:
$A$ **dipende parzialmente** da una chiave $K$ se $\exists X \subset R$ tale che $X \to A \in F^+$ con $A \notin X$ e tale che $X \subset K$ e $A$ non è parte di una chiave
# dipendenza transitiva
$X \to A \in F^+, A \notin X$ è una dipendenza transitiva su R se $A$ non è primo e per ogni chiave $K$ di R si ha che $X$ **non** è contenuto propriamente in $K$ e $K-X \neq \varnothing$
>[!figure] ![[Pasted image 20241024222117.png|400]]

>[!example]
>$\text{Studente(\textbf{Matr}, Cogn, Nome, Data, Com, Prov)}$
ad un numero di matricola corrisponde solo un comune di nascita ( quello dello studente con quel numero di matricola): $Matr \to Com$
inoltre, un comune si trova in una sola provincia ($Com \to Prov$)
quindi, per transitività, esiste la dipendenza funzionale $Matr \to Prov$, che è conseguenza delle 2 dipendenze funzionali descritte sopra, e quindi si può dire che l’attributo $Prov$ dipende transitivamente dalla chiave $Matr$
$Com \to Prov$ viene detta dipendenza transitiva 
(in soldoni: la dipendenza che ci permette di applicare la transività che però non ha la determinante non chiave)

dal punto di vista dell’attributo che dipende transitivamente:
$A$ **dipende transitivamente** da una chiave $K$ se $\exists X \subset R$ tale che $K \to X \in F^+$ con $A \notin X$ e $X \to A \in F^+$ e $X$ non è una chiave, ed $A$ non è parte di una chiave

>[!info] uhhh
>siamo studiando le relazioni dei dipendenti quando non sono determinati da una chiave completa ! (fose tipo)

# definizione alternativa di 3FN
dato uno schema R e un insieme di dipendenze funzionali F, R è in 3FN se e solo se non ci sono attributi che dipendono parzialmente o transitivamente da una chiave
## dimostrazione
###  schema in 3FN $\implies$ no dipendenze parziali o transitive
abbiamo 2 possibilità per ogni dipendenza funzionale in $F^+$:
- $A$ è primo: $\implies$ viene a mancare la prima condizione per avere una dipendenza parziale o transitiva
- $A$ non è primo: allora $X$ è una superchiave, e in quanto tale **può** contere una chiave al suo interno, ma non può essere contenuta propriamente in una chiave(altrimenti la chiave che contiene $X$ non sarebbe una chiave, ma una superchiave), quindi non si può verificare una dipendenza parziale. Inoltre, essendo superchiave, non può verificarsi che, per ogni chiave $K$ di R, $X$ non è contenuto propriamente in $K$ e $K - X \neq \varnothing$($X$ contiene completamente almeno una chiave di R). quindi non si può verificare neanche una dipendenza transitiva
###  no dipendenze parziali o transitive$\implies$schema in 3FN
Suppongo **per assurdo** che anche se non esistono dipendenze parziali o transitive, lo schema **non è in 3FN**
ciò vuol dire che esiste almeno una dipendenza che viola la 3FN ($A$ non è primo e $X$ non è una superchiave)
ci sono 2 casi(valutiamo X, che per ipotesi **non può essere una superchiave**):
- per ogni chiave $K$ di R, $X$ non è contenuto propriamente in nessuna chiave, e $K - X \neq \varnothing$ . ciò implica l’esistenza di una **dipendenza transitiva**
- esiste una chiave $K$ di R tale che $X \subset K$: in tal caso $A$ non può essere primo(per violare la 3FN), e quindi $X \to A$ è una **dipendenza parziale**
quindi, la dimostrazione per assurdo è completata

>[!warning] se tutte le dipendenze hanno come determinante una superchiave, non posso violare le dipendenze funzionali