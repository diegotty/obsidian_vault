---
created: "2024-10-31"
related to: 
updated: "2024-10-31, 08:42"
---
come abbiamo già visto, quando si decompone uno schema di relazione $R$ su cui si è definito un insieme di dipendenze funzionali $F$, oltre ad ottenere schemi in 3FN, occorre:
- preservare le dipendenze in $F^+$
- poter ricostruire tramite join naturale tutta e solo l’informazione originaria (senza perdita di informazioni)
si è quindi interessati a calcolare $F^+$, e sappiamo come farlo, ma ciò al momento ci richiede tempo **esponenziale** in $|R|$
>[!info] fortunantamente, per i nostri scopi è sufficiente avere un metodo per decidere se una dipendenza funzionale $X \to Y$ appartiente ad $F^+$
>- ciò può essere fatto calcolando $X^+$ e verificando se $Y \subseteq X^+$, infatti, per il **lemma 1**: $X \to Y \in F^A \iff Y \subseteq X^+$, e noi sappiamo che $F^A = F^+A$

vedremo che il calcolo di $X^+$ è utile in diversi casi … 
# algoritmo per calcolo di $X^+$
**input**: uno schema di relazione R, un insieme $F$ di dipendenze funzionali su $R$, e un sottoinsieme $X$ di $R$ ($X$ può essere un singolo attributo)
**output**: la chiusura di $X$ rispetto ad $F$ (restituita nella variabile $Z$)

**begin**
$Z:= X;$
$S := \{A/Y \to V \in F \land A \in V \land Y \subseteq Z\}$
**while**($S \not\subset Z$)
	**do**	
	$Z := Z \cup S;$
	$S := \{A/Y \to V \in F \land A \in V \land Y \subseteq Z\}$
**end**
>[!info] spiegazione
ad ogni iterazione, in S inserisco i **singoli** attibuti ($A$) che compongono la dipendente ($V$) delle dipendenze in $F$ la cui determinante è contenuta in $Z$
all’inizio $Z :=X$, quindi inserisco in $S$ tutte le dipendenti che sono determinate da $X$. nelle iterazioni successive, avendo aggiunto elementi, potrei trovare nuove dipendenze la cui determinante è contenuta in $Z$, che, per transività, saranno dipendenti anche da $X$ !
l’algoritmo si ferma quando il nuovo insieme $S$, appena calcolato, è già completamente contenuto nell’insieme $Z$, cioè quando non possiamo aggiungere nuovi attributi alla chiusura transitiva di $X$

>[!example] esempio dell’algoritmo
$F = \{AB \to C, B \to D, AD \to E, CE \to H\}$
$R = ABCDEHL$
vogliamo calcolare la chiusura di $AB$
>
>$i = 0$
>$Z = AB, S = \{C, D \}$, perchè $AB \to C \in F$, e applicando la transitività su $AB \to B$(riflessività) e $B \to D \in F$, ho $AB \to D$
>
>$i=1$
>$Z = ABCD, S =\{C,D,E\}$, perchè applicando la transitività su $AB \to AD$(trovata applicando l’aumento su $AB \to D$, la dipendenza che abbiamo trovato anche all’iterazione precedente) e $AD \to E \in F$, ho $AB \to E$
>
>$i = 2$
>$Z=ABCDE, S = \{C,D,E,H\}$, perchè applicando la transitività su $AB \to CE$ (trovata applicando l’unione tra $AB \to E$(calcolata nell’iterazione precedente) ed $AB \to C$), ho $AB \to E$, e $CE \to H$, ho $AB \to H$
$i = 3$
$Z = ABCDEH, S= CDEH$, e non posso aggiungere altro, quindi l’algoritmo si ferma
>
> abbiamo trovato che $AB$ è una superchiave ! per dimostrare che $AB$ è una chiave, devo calcolare le chiusure di $A$ e $B$

# teorema
indichiamo con $Z^{(0)}$ il valore iniziale di Z (quindi $Z^{(0)} = X$ ) e con $Z^{(i)}$  ed $S^{(i)}$ , con $i ≥ 1$, i valori di $Z$ ed $S$ **dopo** l’i-esima esecuzione del ciclo. è facile vedere che $Z^{(0)} \subseteq Z^{(i+1)}, \forall i$
sia $f$ tale che $S^{(f)} \subseteq Z^{(f)}$ (cioè, l’algoritmo è terminato); proveremo che:
>[!info] teorema
>l’algorimo “Calcolo di $X^+$” calcola correttamente la chiusura di un insieme di attributi $X$ rispetto ad un’insieme $F$ di dipendenze funzionali, cioè:
> $$ Z^{(f)} \subseteq X^+ \land X^+ \subseteq Z^{(f)}$$
>e quindi
>$$A \in Z^{(f)} \iff A \in X^+$$

# dimostrazione
## $A \in Z^{(f)} \implies A \in X^+$
visto che $Z^{(f)}$ è generato iterativamente, usiamo una dimostrazione per induzione
caso base : $i = 0$
$Z^0 = X$, quindi $X \subseteq X^+ \implies Z^0 \subseteq X^+$
ipotesi induttiva: $i -1$


