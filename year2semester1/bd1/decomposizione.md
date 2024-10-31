---
created: "2024-10-31"
related to: 
updated: "2024-10-31, 07:43"
---
# decomposizione
se uno schema non è in 3FN, esso può essere **decomposto** per creare un insieme di schemi in 3FN.
esistono diversi modi per decomporre uno schema, ma è necessario che ogni dipendenza originaria (in $F$) sia rispettata negli schemi decomposti, anche se gli attributi interessati dalle dipendenze funzionali sono distribuiti in relazioni diverse.

>[!example] esempio 1
Consideriamo lo schema R=$ABC$ , con l’insieme di dipendenze funzionali $F=\{A \to B, B \to C\}$
lo schema non è in 3FN per la presenza in $F^+$ della dipendenza transitiva $B \to C$, dato che la chiave è evidentemente $A$
$R$ può essere decomposto in:
>	$R1 = AB$ con $\{A \to B\}$ e $R2 = BC$ con $\{B \to C\}$
>**oppure**
>	$R1 = AB$ con $\{A \to B\}$ e $R2 = AC$ con $\{A \to C\}$
>entrambi gli schemi sono in 3FN, tuttavia la seconda soluzione non è soddisfacente, infatti:
>consideriamo due istanze legali degli schemi ottenuti	
![[Pasted image 20241031075006.png]]
l’istanza dello schema originario R che posso ricostruire da questa (e l’unico modo per ricostruirla è facendo un join naturale) è la seguente:
![[Pasted image 20241031075109.png]]
**MA** questa non è un’istanza legale di R, in quanto non soddisfa la dipendenza funzionale $B \to C$( che anche se transtiva, va soddisfatta comunque!!)
**occorre quindi preservare TUTTE le dipendenze in $F^+$**

>[!example] esempio 2
Consideriamo lo schema R=$ABC$ , con l’insieme di dipendenze funzionali $F=\{A \to B, C \to B\}$
lo schema non è in 3FN per la presenza in $F^+$ della dipendenze parziali $A \to B$ e $C \to B$, dato che la chiave è evidentemente $AC$(applico aumento con $A$ sulla dipendenza $C \to B$)
$R$ può essere decomposto in:
>	$R1 = AB$ con $\{A \to B\}$ e $R2 = BC$ con $\{C \to B\}$
tale schema, pu preservando tutte le dipendenze in $F^+$, non è soddisfacente, infatti:
consideriamo l’istanza **legale** di R
![[Pasted image 20241031080146.png]]
in base alla decomposizione data, questa istanza si decompone in:
![[Pasted image 20241031080232.png]]
e, in teoria, dovrebbe essere possibile ricostruirla **esattamente** tramite join naturale.
invece, effettuando il join tra le due istanze **legali** risultanti della decomposizione, si ottiene:
![[Pasted image 20241031080346.png]]
** occorre quindi garantire che il join delle istanze risultanti dalla decomposizione non riveli <u>perdita di informazione</u>** 

>[!example] esempio 3
Consideriamo lo schema R=$ABC$ , con l’insieme di dipendenze funzionali $F=\{\varnothing \}$ (lo schema è ovviamente in 3FN (e ovviamente $F^+$ non è vuoto !!!))
$R$, per motivi pratici può essere decomposto in:
>	$R1 = AB$ con $\{\varnothing\}$ e $R2 = BC$ con $\{\varnothing\}$
tale schema, pu preservando tutte le dipendenze in $F^+$(che sono quelle banali), non è soddisfacente, infatti:
consideriamo l’istanza **legale** di R:
![[Pasted image 20241031080836.png]]
