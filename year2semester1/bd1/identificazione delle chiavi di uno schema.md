---
created: "2024-11-07"
related to: 
updated: "2024-11-07, 07:42"
---

>[!example] esempio 1
dato il seguente schema di relazione:
$R = (A,B,C,D,E, H)$
e il seguente insieme di dipendenze funzionali:
$F = \{AB \to CD, C \to E, AB \to E, ABC \to D \}$
**calcolare la chiusura dell’insieme $ABH$**
>
>applicando l’arlgoritmo per il calcolo della chiusura di un insieme di attributi, inizializziamo $Z=ABH$, e inseriamo in $S=CDE$ . alla prima iterazione abbiamo già aggiunto tutti gli attributi dello schema in $Z$, e abbiamo quindi verificato $$(ABH)^+ = \{A,B,C,D,E,H\}$$
>
>$ABH$ potrebbe essere una chiave ? devono essere vere le condizioni: 
>1. $K \to R \in F^+$
>2. non esiste un sottoinsieme proprio $K’$ di $K$ tale che $K’ \to R \in F^+$

>[!info] osservazioni
>1. conviene partire dai sottoinisiemi di $ABH$ di cardinalità maggiore: se la loro chiusura non contiene $R$, è inutila calcolare la chiusura dei loro sottoinsiemi
>2. gli attributi che **non compaiono mai a destra delle dipendenze funzionali di F**, non sono determinati funzionalmente da nessun attributo, quindi **dovranno essere sicuramente in ogni chiave** (perchè rimarebbero fuori da qualunque chiusura transitiva, e una chiave deve determinare l’intero schema)
>3. gli attributi che compaiono sempre e solo come dipendenti non potranno stare in una chiave
>4. gli attributi che non compaiono mai( nè determinante, nè dipendente) nelle dipendenze funzionali in $F$, devono stare per forza nella chiave

>[!example] continuo dell’esempio 1
verifichiamo che $ABH$ è chiave: calcoliamo la chiusura dei suoi sottoinsiemi.
usando le osservazioni appena elencate, notiamo che $H$ non appare mai come dipendente (o determinante), quindi sicuramente farà parte della chiave. dobbiamo quindi calcolare la chiusura di $AH$ e $BH$
**blablabla non entriamo neanche nella prima iterazione. lame. L + not keys**
quindi inutile provare con sottoinsiemi di cardinalità minore. 
**$ABH$ è una chiave di $R$**
inoltre, tentando le chiusure di altri sottoinsiemi di $R$, si può arrivare alla conclusione che **$ABH$ è l’unica chiave**

dato uno schema $R$ ed un insieme di dipendenze funzionali su $R$, **possiamo avere più chiavi per $R$ !**

>[!example] esempio 2
dato il seguente schema di relazione:
$R = (A,B,C,D,E, G, H)$
e il seguente insieme di dipendenze funzionali:
$F = \{AB \to D, G \to A, G \to B, H \to E, H \to G, D \to H \}$
**determinare le 4 chiavi di $R$**

cominciamo dagli attributi che ne determinano funzionalmente altri , e quelli che 
