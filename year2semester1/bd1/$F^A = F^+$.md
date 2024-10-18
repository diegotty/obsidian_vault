---
created: "2024-10-18"
related to: 
updated: "2024-10-18, 16:04"
---
# teorema
>[!note] teorema
Siano R uno schema di relazione ed F un insieme di dipendenze funzionali su R. si ha $F^A = F^A$, cioè
$$F^A \subseteq F^+ \land F^+ \subseteq F^A$$

dimostriamo quindi il teorema dimostrando la doppia inclusione di sopra, che singifica:
- ogni dipendenza ottenibile dagli assiomi di Armstrong deve essere necessariamente soddisfatta da ogni istanza legale
- tutte le dipendenze che devono essere soddisfatte da un’istanza legale si possono ricavare con gli assiomi di Armstrong
# dimostrazione
## $F^+ \supseteq F^A$
data una dipendenza $X \to Y \in F^A$ dimostriamo che $X \to Y \in F^+$ per induzione sul numero $i$ di applicazioni di **uno degli assiomi di Armstrong**
caso base: $i=0$ 
- $X \to Y \in F^+$, quindi, visto che $F \subseteq F^+$, $X \to Y \in F^+$
ipotesi induttiva: $i-1$
- $X \to Y \in F^A \implies X \to Y \in F^+$, quindi ($X \to Y$ è soddisfatta da ogni istanza legale)
passo induttivo, $i$:
- $X \to Y \in F^A$
- diciamo che $X \to Y$ non è altro che la dipendenza funzionale ottenuta applicando uno degli assiomi di Armstrong $i-1$ volte(la dipendenza funzionale dell’ipotesi induttiva), a cui applichiamo uno degli assiomi di Armstrong.
- abbiamo quindi 3 casi, uno per ogni assioma che potremmo aver applicato alla dipendenza funzionale dell’ipotesi induttiva.
1. **assioma della riflessività** ipotizziamo di aver usato l’assioma della riflessività sull’ipotesi induttiva. **applichiamo quindi l’assioma al contrario**, quindi sappiamo che: $Y \subseteq X$. ciò vuol dire che, $\forall r \text{, (r istanza legale di R)}, t_{1}[X]=t_{2}[X] \implies t_{1}[Y]=t_{2}[Y])$
2. **assioma dell’aumento** ipotiziamo di aver usato l’assioma dell’aumento sulla dipendenza funzionale ottenuta in $i-1$ passi, che per comodità chiamiamo $V \to W \in F^A$(che per ipotesi induttiva, $\in F^+$). possiamo quindi ricavare che $X = VZ, Y=WZ$