---
created: 2024-11-09
related to: "[[decomposizione]]"
updated: 2024-11-09, 11:17
---
come abbiamo visto nelle lezioni precedenti, esistono delle [[decomposizione#condizioni della decomposizione|condizioni per effettuare una decomposizione correttamente]]. 
il nostro obiettivo è creare delle decomposizioni che rispettano le condizioni
per fare ciò, diamo una definizione rigorosa di decomposizione:
>[!info] definizione
Sia R uno schema di relazione. Una decomposizione di $R$ è una famiglia $\rho = \{R_1, R_2, …, R_K\}$ di sottoinsiemi di $R$ che ricopre $R$ $(\cup^K_{i=1} R_{i}=R)$ ( i sottoinsiemi possono avere intersezione non vuota)
>- in altre parole: decomporre lo schema $R$ significa definire dei sottoschemi che contengono, ognuno, un sottoinsieme di $R$. I sottoschemi possono avere attributi in comune, e la loro unione deve **necessariamente** contenere **tutti** gli attributi di $R$.

decomporre un’istanza di una relazione con un certo schema, vuol dire, per ogni sottoschema della decomposizione, proiettare le tuple dello schema originale sugli attributi dei sottoschemi.

# definizione formale di preservamento di dipendenze
>[!info] definizione
sia $R$ uno schema di relazione, $F$ un insieme di dipendenze funzionali su $R$ e sia $\rho = \{R_{1}, R_{2}, \dots, R_{k}\}$ una decomposizione di $R$
diciamo che $\rho$ preserva $F$ se $F \equiv 

# lemma 2
siano $F$ e $G$ due dipendenze funzionali. 
$$F \subseteq G^+ \implies F^+ \subseteq G^+$$
( sarebbe un $\iff$ ma l’altra parte dell’implicazione è banale)
>[!info] info
>con questo lemma possiamo ridurre le cose da verificare per verificare che $G^+ = F^+$
>- al posto di calcolare $F^+ \subseteq G^+$, calcolo $F \subseteq G^+$, cioè, per il lemma 1:
$\forall X \to Y \in F$, cerco se $Y \subseteq X^+_G$ se ciò è vero, $X \to Y \in G^A(=G^+)$
>- al posto di calcolare $G^+ \subseteq F^+$, calcolo $G \subseteq F^+$, quindi, per il lemma 1: 
>$\forall X \to Y \in G$, cerco se $Y \subseteq X^+_F$ se ciò è vero, $X \to Y \in F^A(=F^+)$