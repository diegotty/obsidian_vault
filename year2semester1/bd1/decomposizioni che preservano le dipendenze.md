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