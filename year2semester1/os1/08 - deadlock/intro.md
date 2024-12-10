---
created: 2024-12-10
related to: "[[intro alla concorrenza]]"
updated: 2024-12-10T09:24
---
**deadlock**: blocco permanente di un insieme di processi, che competono per delle risorse di sistema o comunicano tra loro
- il motivo di base è la richiesta contemporanea delle stesse risorse da parte di due o più processi !
- non esiste una soluzione efficace, ma esistono diversi approcci, da valutare nei vari casi
- anche se il deadlock potrebbe essere causato da un **corner case** (una combinazione rara di eventi),  va comunque prevenuto 
>[!example] esempio di dealock
![[Pasted image 20241210083651.png]]
>in questo esempio (a) esistonon tante combinazioni possibili che risultano in una gestione corretta, ma ne esiste anche uno che finisce in deadlock (b)
>- if you do this you are braindead. and i hate you
## joint progress diagram
utilizzato per gestire deadlock tra pochi processi(2,3 procesi, xke 1 processo per asse)
>[!example] esempio 1
>![[Pasted image 20241210084017.png]]
x-axis: progress of P 
y-axis: progress of Q
le linee marcate in nero sono le possibili progress path di P e Q
se il dispatcher gestisce i processi in modo tale da seguire la linea (3) o (4), è assicurato un deadlock (che avviene nelle zone a righe, non nella zona grigia ! però, per arrivare nelle zone a righe, si ““entra”” solo dalla zona grigia(?))

>[!example] esempio 2
![[Pasted image 20241210090428.png]]
in questo caso invece, non si arriverà mai a un deadlock (infatti le zone a righe esistono, ma non esiste una zona grigia attraverso cui arrivarci)
>- in questo caso, P è molto più “generoso” di prima, in quanto non necessita di possedere 2 risorse contemporaneamente

## risorse riusabili
usabili da un solo processo alla volta,  e il fatto di essere usate non le “consuma”. una volta che un processo ottiene una risorsa riusabile, prima o poi la rilascia, cosicchè altri processi possano usarla a loro volta
- es: processori, I/O channels, memoria primaria/secondaria, dispositivi, file, database, etc
per le risorse riusabili, lo stallo può esistere solo se un processo ha una risorsa e ne richiede un’altra
>[!example] esempio 1 con 2 processi
![[Pasted image 20241210091149.png]]
perform funzion: sezione critica
i lock sono semplificazioni dei semafori (`request(D) + lock(D) == semWait(D)`)

>[!example] esempio 2 con 2 processi
![[Pasted image 20241210091533.png]]
supponiamo di avere 200KB di memoria disponibili:
>- il deadlock avverà quando uno dei due processi farà la seconda richiesta (se anche l’altro processo ha effettuato la prima richiesta)
>- in questo esempio, se P1 venisse eseguito nella sua interezza, la memoria usata e poi rilasciata da P1 può essere **riusata** da P2


## risorse consumabili
vengono create(prodotte) e distrutte(consumate)
- es: interruzioni, segnali, messaggi, informazione dei buffer di I/O
in questo caso il deadlock si verifica se si fa una richiesta (bloccante) di una risorsa non ancora creata
>[!example] esempio con 2 procesi
![[Pasted image 20241210091941.png]]
who made this mf burger dawg
# grafo dell’allocazione delle risorse
grafo diretto che rappresenta lo stato di risorse e processi
- tanti pallini quante istanze di una stessa risorsa