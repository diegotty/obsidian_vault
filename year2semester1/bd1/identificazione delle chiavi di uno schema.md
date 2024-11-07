---
created: "2024-11-07"
related to: 
updated: "2024-11-07, 07:42"
---

>[!example] esempio 1
dato il seguente schema di relazione:
$R = (A,B,C,D,E)$
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
>2. gli attributi che **non compaiono mai a destra delle dipendenze funzionali di F**, non sono determinati funzionalmente da nessun attributo