---
created: 2024-11-27
related to: "[[17, 18 - copertura minimale di un insieme di dipendenze]]"
updated: 2024-11-27T06:22
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
		\State $S := S \cup {A};$
    \EndFor
	\If{ S \neq \varnothing}
		\State $R:=R - S;$
		\State $\rho =_
    \EndIf

	\end{algorithmic}
	\end{algorithm}
```