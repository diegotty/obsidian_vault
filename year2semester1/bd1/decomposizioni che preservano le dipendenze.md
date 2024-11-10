---
created: 2024-11-09
related to: "[[decomposizione]]"
updated: 2024-11-09, 11:17
---
come abbiamo visto nelle lezioni precedenti, esistono delle [[decomposizione#condizioni della decomposizione|condizioni per effettuare una decomposizione correttamente]]. 
il nostro obiettivo è creare delle decomposizioni che rispettano le condizioni
per fare ciò, diamo una definizione rigorosa di decomposizione:
>[!info] definizione
Sia R uno schema di relazione. Una decomposizione di $R$ è una famiglia $\rho = \{R_1, R_2, …, R_K\}$ di sottoinsiemi di $R$ che ricopre $R$ $(\bigcup_{i=1}^{K} R_{i}=R)$ ( i sottoinsiemi possono avere intersezione non vuota)
>- in altre parole: decomporre lo schema $R$ significa definire dei sottoschemi che contengono, ognuno, un sottoinsieme di $R$. I sottoschemi possono avere attributi in comune, e la loro unione deve **necessariamente** contenere **tutti** gli attributi di $R$.

decomporre un’istanza di una relazione con un certo schema, vuol dire, per ogni sottoschema della decomposizione, proiettare le tuple dello schema originale sugli attributi dei sottoschemi.
# definizione formale di preservamento di dipendenze
>[!info] definizione
sia $R$ uno schema di relazione, $F$ un insieme di dipendenze funzionali su $R$ e sia $\rho = \{R_{1}, R_{2}, \dots, R_{k}\}$ una decomposizione di $R$
diciamo che $\rho$ preserva $F$ se $F \equiv \bigcup_{i=1}^K \,\pi_{R_i} (F)$
dove $\pi_{R_i}(F)=\{X \to Y | X \to Y \in F^+ \land XY \subseteq R_i\}(=G)$ 

## $\pi_{R_i}(F)$
$\bigcup_{i=1}^K \,\pi_{R_i} (F)$ e un insieme di dipendenze funzionali, dove ogni $\pi_{R_i}$ è un insieme di dipendenze funzioali dato dalla proiezione dell’insieme di dipendenze funzionali $F$ sul sottoschema $R_i$, che significa: 
- prendere tutte e sole le dipendenze **derivabili** da $F$ tramite gli assiomi di Armstrong (quindi quelle in $F^+$), che hanno tutti gli attributi (determinante e dipendente) in $R_i$

## verificare l’equivalenza tra 2 insiemi di dipendenze funzionali
dobbiamo quindi verificare che $F \equiv G$.
>[!info] definizione
>Siano F e G due insiemi di dipendenze funzionali, $F$ e $G$ sono equivalenti( $F \equiv G$) se $F^+ = G^+$
>- quindi non è necessario, che $F$ e $G$ contengano le stesse dipendenze, bensì che le chiusure di essi contengano le stesse dipendenze

come sappiamo, per verificare l’uguaglianza tra due insiemi $\iff$ contenimento nei due versi, quindi bisogna verificare che:
$$F^+ \subseteq G^+ \land G^+ \subseteq F^+$$
notiamo che per come è stato definito $G$ **in questo caso**(??), sarà sicuramente vero $F^+ \supseteq G$, in quanto $G$ è l’unione di sottoinsiemi di di $F$
# lemma 2 (lemma delle chiusure)
siano $F$ e $G$ due dipendenze funzionali. 
$$F \subseteq G^+ \implies F^+ \subseteq G^+$$
( sarebbe un $\iff$ ma l’altra parte dell’implicazione è banale)
>[!info] info
>con questo lemma possiamo ridurre le cose da verificare per verificare che $G^+ = F^+$
>- al posto di calcolare $F^+ \subseteq G^+$, calcolo $F \subseteq G^+$, cioè, per il lemma 1:
$\forall X \to Y \in F$, cerco se $Y \subseteq X^+_G$ se ciò è vero, $X \to Y \in G^A(=G^+)$
>- al posto di calcolare $G^+ \subseteq F^+$, calcolo $G \subseteq F^+$, quindi, per il lemma 1: 
>$\forall X \to Y \in G$, cerco se $Y \subseteq X^+_F$ se ciò è vero, $X \to Y \in F^A(=F^+)$

>[!info] dimostrazione
sia $f \in F^+ - F$, cioè una dipendenza che compare in $F^+$ ma non in $F$
>- per ipotesi del lemma, $F \subseteq G^+$, quindi ogni dipendenza funzionale si può ricavare da $G$ (applicando gli assiomi di Armstrong su $G$, arrivo a $G^+$, e nel “tragitto” avrò quindi ricavato ogni dipendenza in $F$) (inoltre abbiamo dimostrato che $F^+=F^A$, altrimenti se $F^+ \neq $F^A$ questo passo non funzionerebbe)
> - sempre per il teorema che dimostra che $F^+ = F^A$, sappiamo che $F \in F^+$ è ricavabile da $F$ applicando gli assiomi di Armstrong. 
> - quindi, $f$ è derivabile da $G$ mediante gli assiomi di armstrong:
> $$G \to^A F \to^A F^+$$
(con $\to^A$ denotiamo la derivazione tramite gli assiomi di Armstrong)



quindi, per il lemma delle chiusure, visto che sappiamo che $G \subseteq F^+ \implies G^+ \subseteq F^+$
- per verificare che $G^+=F^+$, dobbiamo solo dimostrare che $F^+ \subseteq F^+$
- le dipendenze che ci danno problemi nel verificare $F^+ \subseteq G^+ sono le dipendenze $X \to Y \in F$ in cui $X$ e $Y$ non sono mai contenute insieme in un sottoschema (sono quindi, “a cavallo” di due tabelle)
# verifica di $F \subseteq G^+$
la verifica può essere fatta con il seguente algoritmo:
**algoritmo - contentimento di $F$ in $G^+$**
**input** : due insiemi $F$ e $G$ di dipendenze funzionali su $R$
**output** : la variabile `successo`, che al termine avrà valore `true` se $F \subseteq G^+$, `false` altrimenti
```pseudo
	\begin{algorithm}
	\caption{verifica contenimento di $F$ in $G^+$}
	\begin{algorithmic}
		\State success := true;
		\ForAll{$X \to Y \in F$}
			\State calcola $X^+_G$;
			\If{ $Y \not\subset X^+_G$} 
			\State successo := false
			\EndIf
		\EndFor
	\end{algorithmic}
	\end{algorithm}
```
la correttezza dell’algoritmo è una conseguenza del lemma 1 e del teorema $F^+ = F^A$, infatti: 
- al posto di calcolare $F^+ \subseteq G^+$, calcolo $F \subseteq G^+$, cioè, per il lemma 1: $\forall X \to Y \in F$, $Y \subseteq X^+_G \iff X \to Y \in G^A(=G^+)$
>[!warning] basta verificare che anche sola una dipendenza non appartiene alla chiusura di G per poter affermare che l’equivalenza non sussiste !

come calcoliamo $X^+_G$ ?
- se volessimo usare l’[[chiusura di un insieme di attributi#algoritmo per calcolo di $X +$| algorimo per il calcolo della chiusura di un insieme di attributi]] , dovremmo prima calcolare $G$, ma per la definizione di $G$, ciò richiede il calcolo di $F^+$, che richiede tempo esponenziale. 
usiamo quindi il seguente algoritmo, che permette di calcolare $X^+_G$ a partire da $F$
## algorimo calcolo $^+_G$ a partire da $F$
**input** : uno schema $R$, un insieme $F$ di dipendenze funzionali su $R$, una decomposizione $\rho = \{R_{1}, R_{2}, \dots, R_{k}\}$di $R$, un sottoinsieme di $X$  di $R$;
**output** la chiusura di $X$ rispetto a $G = \bigcup_{i=1}^k \pi_{R_i}(F)$, nella variabile Z;
```pseudo
	\begin{algorithm}
	\caption{calcolo di $X^+_G$ a partire da $F$}
	\begin{algorithmic}
		\State $Z := X;$
		\State $S := \varnothing$;
		\For{$i:=1$ to $k$}
			\State $S := S \cup(Z \cap R_i)^+_F \cap Ri$
		\EndFor
		\While{ $S \not\subset Z$}
			\State $Z := Z \cup S$
			\For{$i:=1$ tok $k$}
				\State $S := S \cup(Z \cap R_i)^+_F \cap Ri$
			\EndFor
		\EndWhile
	\end{algorithmic}
	\end{algorithm}
```
 $S:= S \cup(Z \cap R_i)^+_F \cap Ri$ : calcolo la chiusura su F degli attributi presenti in $F$ e allo stesso tempo in $R_i$ (quindi tutte le dipendenti delle dipendenze in $F$, che hanno come determinante $Z \cup R_i$ ) e interseco la chiusura con $R_i$, in modo da avere la chiusura di $X$ per $R_i$. se si esce dal while in anticipo, facciamo questo procedimento per ogni $R_i$, quindi in $S$ stiamo accumulando gli attributi di tutti i sottoschemi che sono determinati funzionalmente da $X$
