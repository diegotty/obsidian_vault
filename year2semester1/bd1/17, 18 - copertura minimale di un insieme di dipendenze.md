---
created: 2024-11-21
related to: 
updated: 2024-11-26T20:52
---
>[!index]
>
>- [$F^{min}$](#$F%5E%7Bmin%7D$)
>- [come calcolare $F^{min}$](#come%20calcolare%20$F%5E%7Bmin%7D$)
>- [esempi](#esempi)

affrontiamo ora il problema di come ottenere una decomposizione che soddisfi le nostre condizioni.
>[!warning] è possibile sempre ottenerla ? si !
> ed esiste un algoritmo che raggiunge questo scopo
# $F^{min}$
dato un insieme di dipendenze funzionali $F$, una copertura minimale di $F$ è un insieme $G$ di dipendenze funzionali equivalente a F ($F \equiv G$), tale che:
-  per ogni dipendenza funzionale in $G$, la parte destra è un **singleton** (cioè è costitutita da un solo attributo)
- per nessuna dipendenza $X \to A \in G$, esiste $X’ \subset X$ tale che $G \equiv G - \{X \to A\} \cup \{X’ \to A\}$
- per nessuna dipendenza funzionale $X \to A \in G$, $G \equiv G - \{X \to A\}$ (ogni dipendenza non è ridondante)
# come calcolare $F^{min}$
1. usando la regola della decomposizione, si riducono a singleton le parti destre delle dipendenze funzionali
2. $\forall X \to A \in F$, verifichiamo se $\exists X’ \subset X \,\,t.c. \,\ F \equiv G$, dove $G = \{X \to A\} \cup \{X’ \to A\}$
	- per dimostare che $F \equiv G$, dobbiamo verificare se  $F^+ \subseteq G^+ \land G^+ \subseteq F^+$, cioè per il lemma 2, dobbiamo dimostare $F \subseteq G^+ \land G \subseteq F^+$
	- inoltre, sappiamo che $G$ e $F$ differiscono solo perchè $X \to A \in F$, mentre $X \to A \not\in G, X' \to A \in G$
	- quindi dobbiamo controllare solo se: 
		- $X \to A \in G^+$, quindi usando il lemma 1, dobbiamo verifcare se $A \in X^+_G$
		- $X’ \to A \in F^+$, quindi usando il lemma 1, dobbiamo verificare se $A \in X’^+_F$
	- $X \to A \in G^+$ non è necessario da calcolare, in quanto $X’ \subseteq X$, quindi per come è costruito l’algoritmo per il calcolo della chiusura, $A$ viene inserito subito ! prima ancora di inziare il while.
	- quindi devo solo controllare se $A \in X’_F$, e se ciò è vero, allora posso ridurre $X \to A$ a $X’ \to A$, e considerare $G$ come l’insieme di riferimento per le verifiche successive
3. $\forall  X \to A$, verificare se $F \equiv F - \{X \to A\} = G$
$X^+_F$ dovrebbe essere uguale a $X^+_G$, in particolare dobbiamo controllare se $X \to A \in G^+$, cioè per il lemma 1, se $A \in X^+_G$. se ciò è vero, $X \to A$ viene eliminata\toA  A $ viene eleminata
>[!info] osservazione
>nel passo 2, se $F$ contiene sia $AB \to C$ che $A \to C$, allora $AB \to C$ non solo si può ridurre, ma si può anche eliminare
nel passo 3, se $X \to A$ ma **non esiste** $Y \to A \in F$ con $Y \not= X$, allora **è inutile provare a eliminare** $X \to A$ !!


>[!warning] il secondo passo va fatto rigorosamente prima del terzo !!!!!

>[!info] possono esserci + coperture minimali 
>se $AB \to C \in F$ e trovo sia $B \to C$ che $A \to C$ in $F^+$, posso ridurla in una delle due dipendenze, e la scelta porta 2 chiusure minimali diverse !
proprio perchè non esiste **LA** decomposizione giusta, ma ci sono diverse possibilità, potrebbe succedere che la decomposizione da verificare **non sia stata ottenuta** tramite l’algoritmo. ciò non ci autorizza a concludere che la decomposizione da verificare non possegga le proprietà richieste !!
>
> può anche succedere che $F$ sia già in forma minimale, e quindi non ci sia da fare alcuna riduzione
# esempi
>[!example] esempio 1