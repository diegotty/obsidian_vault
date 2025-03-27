---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-27T15:31
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
# protocolli con pipeline
nel **pipelining**, il mittente amette più pacchetti in transito, ancora da notificare

## go back N
l’ack è **cumulativo**: tutti i pacchetti fino al numero di sequenza (escluso) dell’ack sono stati ricevuti correttamente
>[!info] finestra di invio
### dimensione finestra d’invio
possiamo avere una finestra di dimensione $2^m$? no, in quanto potrebbero succedere casini (esempio). deve esere $2^m-1$


se la rete funziona bene, è effettivamente il meccansimo più efficiente
## ripetizione selettiva
cerchiamo di evitare il comportamento del meccanismo GBN
- sembra essere un GBN senza ack cumulativo

timer per ogni pacchetto spedito
più costoso a livello di risorse, e si inviano meno pachetti
### dimensione finestra d’invio e ricezione
possono capitare impicci (slide 31)
prendiamo quind dimensione $2^{m-1}$
## protocolli bidirezionali
ack è piggyback-ato. lo ficco addosso ai pacchetti che devo mandare (se sono sia mittente che destinatario)