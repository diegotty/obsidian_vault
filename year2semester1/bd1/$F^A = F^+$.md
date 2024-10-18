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

1. **assioma della riflessività**
	- ipotizziamo di aver usato l’assioma della riflessività sull’ipotesi induttiva: sappiamo che $Y \subseteq X$ 
	- verifichiamo che $X \to Y$ sia verificata per ogni istanza legale.
		- $\forall r \text{, (r istanza legale di R)}, t_{1}[X]=t_{2}[X] \implies t_{1}[Y]=t_{2}[Y]$
		- per l’ipotesi induttiva, sappiamo che ciò è banalmente verificato (uhhhhhhhh)
		- ciò vale per ogni istanza legale di R, quindi $X \to Y \in F^+$
		
2. **assioma dell’aumento**
	- ipotiziamo di aver usato l’assioma dell’aumento sulla dipendenza funzionale ottenuta in $i-1$ passi, che per comodità chiamiamo $V \to W \in F^A$(che per ipotesi induttiva, $\in F^+$). 
	- possiamo quindi ricavare che $X = VZ , \,\,\, Y=WZ$ (abbiamo moltiplicato i termini dell’ipotesi per un $Z \subseteq R$).
	- verifichiamo che $X \to Y$ sia verificata per ogni istanza legale.
		- $\forall r \text{, (r istanza legale di R)}, t_{1}[X]=t_{2}[X] \implies t_{1}[VZ]=t_{2}[VZ]$
		- possiamo riscriverla in questo modo: $t_{1}[X]=t_{2}[X] \implies t_{2}[V]=t_{2}[V] \land t_{1}[Z]=t_{2}[Z]$
		- per ipotesi induttiva, sappiamo che: $t_{1}[V]=t_{2}[V] \implies t_{2}[W]=t_{2}[W]$
		- sappiamo quindi che $t_{2}[W]=t_{2}[W]$(per ipotesi induttiva) e $t_{1}[Z]=t_{2}[Z]$(per parte sinistra dell’implicazione da dimostrare), quindi $t_{1}[WZ]=t_{2}[WZ]$
		- ciò vale per ogni istanza legale di R, quindi $X \to Y \in F^+$
3. **assioma della transitività**
	- ipotiziamo di aver usato l’assioma dell’additività per ricavare $X \to Y$. ciò vuol dire che abbiamo utilizzato due dipendenze, create in $i-1$ applicazioni di assiomi di Armstrong: $X \to Y \in F^A \,\,\ ,\,\,\,\,Z \to Y \in F^A$, che per ipotesi induttiva $\in F^+$
	- verifichiamo che $X \to Y$ sia verificata per ogni istanza legale.
		- $\forall r \text{, (r istanza legale di R)}, t_{1}[X]=t_{2}[X]$
		- per ipotesi induttiva, $t_{1}[X]=t_{2}[X] \implies t_{1}[Y]=t_{2}[Y]$, e sempre per ipotesi induttiva, $t_{1}[Z]=t_{2}[Z] \implies t_{1}[Y]=t_{2}[Y]$

se prendiamo r non legale non possiamo usare l’ipotesi induttiva ? però è anche importante perchè se prendiamo un’istanza legale dobbiamo dimostrare che la dipendenza del passo induttivo deve essere NECESSARIAMENTE soddisfatta
il ragionamento dovrebbe essere di questo tipo: prendendo una istanza legale r, sapenzo che l’ipotesi induttiva è vera, la dipendenza del passo induttivo deve essere obbligatoriamente soddisfatta. altrimenti r non sarebbe legale. in questo caso, la dipendenza del passo induttivo belongs in $F^+$
