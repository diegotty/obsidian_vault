---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-27T14:28
completed: false
---
>[!warning] stiamo studiando i meccanismi di trasferimento, non dei protocolli ! (penso quindi siano parte di protocolli)
## stop and wait
il meccanismo **stop and wait** è un meccanismo orientato alla connessione, che implementa controllo di flusso e controllo degli errori
il controllo di flusso è intrinseco, in quanto viene inviato un pacchetto alla volta (e si aspetta l’ack per inviare il prossimo, quindi non si sovraccarica mai il destnatario)
### numeri di sequenza
in questo meccanismo, bastano i numeri di sequenza `0` e `1`