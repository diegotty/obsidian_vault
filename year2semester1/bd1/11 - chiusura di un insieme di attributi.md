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
# come si calcola $X^+$
>[!example] algoritmo per calcolo di $X^+$
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
