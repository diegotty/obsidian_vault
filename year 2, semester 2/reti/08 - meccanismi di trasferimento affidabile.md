---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-28T07:10
completed: false
---
>[!warning] stiamo studiando i meccanismi di trasferimento, non dei protocolli ! (penso quindi siano parte di protocolli)
## stop and wait
il meccanismo **stop and wait** è un meccanismo orientato alla connessione, che implementa controllo di flusso e controllo degli errori
- il mittente ed il destinatario usano una [[07 - livello trasporto#integrazione di controllo di errori e controllo di flusso|finestra scorrevole]] di dimensione 1
- quando il pacchetto arriva al destinatario, viene calcolato il checksum:
	- se il pacchetto è corretto, viene inviato l’ack al mittente
	- se il pachetto è **corrotto**, viene scartato senza informare il mittente
- per capire se un pacchetto è andato perso(e non aspettare un ack all’infinito), il mittente usa un timer. se il timer **scade senza ricevere un ack**, il pacchetto viene rinviato
>[!info] illustrazione stop and wait
![[Pasted image 20250328065652.png]]
>- il mittente deve tenere una copia del pacchetto spedito finchè non riceve riscontro
>- il controllo errori viene implementato mediante numero sequenza + ack + timer
>- il controllo di flusso è intrinseco, in quanto viene inviato un pacchetto alla volta (e si aspetta l’ack per inviare il prossimo, quindi non si sovraccarica mai il destinatario)
### numeri di sequenza
per gestire pacchetti duplicati, lo stop&wait(crazy) utilizza i **numeri di sequenza** (in particolare, si vuole identificare l’intervallo più piccolo possibile che possa consentire la comunicazione senza ambiguità) 
>[!example] supponiamo che il mittente abbia inviato il pacchetto con numero di sequenza $x$. si possono verificare tre casi:
>- il pacchetto arriva correttamente al destinatario, che invia un riscontro. il riscontro arriva al mittente che invia il pacchetto successio, $x+1$
>- il pacchetto risulta corrotto o non arriva al destinatario. il mittente, allo scadere del timer, invia nuovamente il pac

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