---
created: 2024-11-27
related to: "[[17, 18 - copertura minimale di un insieme di dipendenze]]"
updated: 2024-11-28T11:07
---
mostriamo ora che dato uno schema di relazione $R$ e un insieme di dipendenze funzionali $F$ su $R$, esiste **sempre** una decomposizione(che può essere calcolata in tempo polinomiale) $\rho = \{R_{1}, R_{2},\dots, R_{k}\}$ di $R$ tale che:
- $\forall i, i=1,\dots,k \,\,R_{i}$ è in 3FN
- $\rho$ preserva $F$
- $\rho$ ha un join senza perdita
# algoritmo per la decomposizione di uno schema

```pseudo
	\begin{algorithm}
	\caption{decomposizione di uno schema di relazione}
	\begin{algorithmic}
	\Input uno schema di relazione $R$ e una copertura minimale $F$
	\Output una decomposizione $\rho$ di $R$ che preserva $F$ e t.c. $\forall i, i=1,\dots,k \,\,R_{i}$ è in 3FN
	\State $S := \varnothing$
	\For{ \textbf{every} $A \in R$ t.c. A non è coinvolto in nessuna dipendenza funzionale in $F$}
		\State $S := S \cup \{A\};$
    \EndFor
	\If{ $S \neq \varnothing$}
		\State $R:=R - S;$
		\State $\rho := \rho \cup \{S\};$
    \EndIf
	\If{esiste una dipendenza funzionale in $F$ che coinvolge tutti gli attributi in $R$}
		\State $\rho := \rho \cup \{R\};$
	\Else \For{\textbf{every} $X \to A \in F$}
		\State $\rho := \rho \cup \{XA\}$
    \EndFor
    \EndIf

	\end{algorithmic}
	\end{algorithm}
```
in questo algoritmo definiamo quanti e come sono fatti gli $R_i$ che compongnono $\rho$. in particolare:
1. nel primo for potremmo aggiungere a $\rho$ un sottoschema composto da tutti gli attributi che non sono conivolti in nessuna dipendenza funzionale in $F$
2. nel secondo if possiamo aggiungere un sottoschema composto da $R - S$ (in cui $S$ è l’insieme composto da tutti gli attributi che non sono coinvolti in nessuna dipendenza di $F$, che potrebbe essere vuoto)
3. altrimenti, nell’else del secondo if andiamo ad aggiungere $n$ sottoschemi ad $\rho$, ed ogni sottoschema è $\{XA\}, \forall X \to A \in F$
>[!info] osservazioni
>possiamo avere 1+2 ($R$ residuo), oppure 1+3, oppure solo 3

# teorema
sia $R$ uno schema di relazione ed $F$ un insieme di dipendenze funzionali su $R$, che è una copertura minimale. L’algoritmo di decomposizione permette di calcolare in tempo polinomiale una decomposizione $\rho$ di $R$ tale che:
- ogni schema di relazione $\rho$ è in 3FN
- $\rho$ preserva $F$
# dimostrazione
dimostriamo separatamente le due proprietà della decomposizione:
- $\rho$ preserva $F$
sia $G= \bigcup_{i=1}^k \pi_{R_i}(F)$. poichè per ogni dipendenza funzionale $X \to A \in F$(**proprio per tutte !**) si ha che $XA \in \rho$ (è proprio uno dei sottoschemi), si ha che questa dipendenza di $F$ sarà sicuramente in $G$, quindi $G \supseteq F$, e quindi $G^+ \supseteq F^+$.
l’inclusione $G^+ \subseteq F^+$ è banalmente verificata in quanto, per definizione $G \subseteq F$

- ogni sottoschema di relazione in $\rho$ è in 3FN
**analizziamo i diversi casi che si possono presentare:**
1.  se $S \in \rho$, ogni attributo di $S$ fa parte della chiave(in quanto $S$ è l’inisieme di attributi non determinati da dipendenze in $F$, e quindi dovranno essere per forza nella chiave che li determinerà per riflessività), e quindi, $S$ è in 3FN ()
2. relazione che comprende tutto $R$, $R-A \to A$
nessun sottoinsieme di $R-A$ può determinare $A$, in quanto siamo nella chiusura minimale, e se la premessa fosse vera, troveremmo il sottoinsieme come determinante nella dipendenza, non $R-A$
(in particolare, sarebbe ridotta nel passo 2 della chiusura minimale)
3. 
anche in questo caso $X$ è chiave per $X \to A$
