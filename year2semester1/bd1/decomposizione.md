---
created: "2024-10-31"
related to: 
updated: "2024-10-31, 07:43"
---
# decomposizione
se uno schema non è in 3FN, esso può essere **decomposto** per creare un insieme di schemi in 3FN.
esistono diversi modi per decomporre uno schema, ma è necessario che ogni dipendenza originaria (in $F$) sia rispettata negli schemi decomposti, anche se gli attributi interessati dalle dipendenze funzionali sono distribuiti in relazioni diverse.

>[!example] esempio 1
Consideriamo lo schema R=$ABC$ , con l’insieme di dipendenze funzionali $F={A \to B, C \to B}$
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
**occorre quindi preservare TUTTE le dipe