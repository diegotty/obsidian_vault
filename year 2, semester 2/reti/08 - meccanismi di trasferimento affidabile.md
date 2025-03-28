---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-28T07:33
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
>- il pacchetto risulta corrotto o non arriva al destinatario. il mittente, allo scadere del timer, invia nuovamente il pacchetto $x$
>- il pacchetto arriva correttamente al destinatario ma il riscontro viene perso o corrotto. scade il timer e il mittente rispedisce $x$. il destinatario riceve un duplicato. **se ne accorge ?**

in questo meccanismo, sono sufficienti i numeri di sequenza `0` e `1`, che vengono usati in questo modo:
- l’`ack` indica il numero di sequenza del prossimo pacchetto atteso dal destinatario (se ha ricevuto il pacchetto 0, invierà ack 1)
### efficienza
con questo meccanismo , il prodotto $\text{rate} \cdot \text{ritardo}$ (cioè la misure del numero di bit che il mittente può inviare prima di ricevere un ack (cioè il volume della pipe in bit)). se il rate è elevato ed il ritardo è consistente, lo stop and wait è inefficiente !
>[!example]- esempio
in un sistema che utilizza stop and wait, abbiamo:
>- rate = 1mbps
>- ritardo di andata e ritorno di 1 bit: 20ms
>
>quanto vale $\text{rate} \cdot \text{ritardo}$ ?
//cba sono preso a male
molto efficiente (non ci permette di utilizzare la rete al meglio)
# protocolli con pipeline
nel **pipelining**, il mittente amette più pacchetti in transito, ancora da notificare
- in questi meccanismi, l’intervallo dei numeri di sequenza deve essere incrementato
- viene effettuato il **buffering**(vengono memorizzati diversi pacchetti in un buffer) dei pacchetti presso il mittente e/o ricevente
>[!figure] illustrazione protocolli con pipeline
![[Pasted image 20250328071913.png]]
## go back N
>[!info] schema generale
![[Pasted image 20250328071957.png]]

nel meccanismo **go back N**:
- i **numeri di sequenza** sono calcolati modulo $2^m$, dove $m$ è la dimensione del campo “numero di sequenza” in bit (huh ?)
- l’ack è **cumulativo**: tutti i pacchetti fino al numero di sequenza (escluso) dell’ack sono stati ricevuti correttamente (ack=7: i pacchetti fino al 6 sono stati ricevuti correttamente, il destinatario attende il 7)
- il mittente mantiene un timer per il più vecchio pacchetto non riscontrato, e allo scadere del timer,**go back N**, cioè vengono rispediti tutti i pacchetti in attesa di riscontro (il destinatario ha finestra di ricezione di dimensione 1, quindi non può bufferizzare i pacchetti fuori sequenza. li devo rinviare tutti)
### finestre di invio e ricezione
>[!info] finestra di invio
![[Pasted image 20250328072405.png]]
la finestra di invio è un concetto astratto, che definisce una porzione immaginaria, con tre variabili: $S_{f}, S_{n}, S_{size}$
>- la finestra di invio può scorrere uno o più posizioni quando viene ricevuto un riscontro privo di errori con `ackNo` maggiore o uguale a $S_{f}$ e minore di $S_{n}$ in artimetica modulare

>[!example]- esempio di scorrimento
![[Pasted image 20250328072642.png]]

intuiamo la dimensione della finestra d’invio: possiamo avere una finestra di dimensione $2^m$? 
no, …….. deve esere quindi $2^m-1$
>[!tip] se la rete funziona bene, questo è effettivamente il meccansimo più efficiente
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