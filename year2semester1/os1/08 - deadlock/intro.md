---
created: 2024-12-10
related to: "[[intro alla concorrenza]]"
updated: 2024-12-10T09:00
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
utilizzato per gestire deadlock tra pochi processi
>[!info] diagramma
>![[Pasted image 20241210084017.png]]
x-axis: progress of P 
y-axis: progress of Q
le linee marcate in nero sono le possibili progress path di P e Q
se il dispatcher gestisce i processi in modo tale da seguire la linea 