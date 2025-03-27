---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-27T14:43
completed: false
---
>[!warning] stiamo studiando i meccanismi di trasferimento, non dei protocolli ! (penso quindi siano parte di protocolli)
## stop and wait
il meccanismo **stop and wait** è un meccanismo orientato alla connessione, che implementa controllo di flusso e controllo degli errori
il controllo di flusso è intrinseco, in quanto viene inviato un pacchetto alla volta (e si aspetta l’ack per inviare il prossimo, quindi non si sovraccarica mai il destnatario)
### numeri di sequenza
in questo meccanismo, sono sufficienti i numeri di sequenza `0` e `1`, che vengono usati in questo modo:
- l’`ack` indica il numer


molto efficiente (non ci permette di utilizzare la rete al meglio)
## protocolli con pipeline
nel **pipelining**, il mittente amette più pacchetti in transito, ancora da notificare

### go back N
l’ack è **cumulativo**: tutti i pacchetti fino al numero di sequenza dell’ack sono stati ricevuti correttamente
>[!info] finestra di invio
