---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-28T19:46
completed: false
---
approfondiamo il protocollo TCP, che è stato introdotto [[07 - livello trasporto#TCP|precedentemente]]. le caratteristche del protocollo TCP sono:
- **protocollo con pipeline**: come [[08 - meccanismi di trasferimento affidabile#protocolli con pipeline|abbiamo visto]], ammette più pacchetti in transito contemporaneamente
- **bidirezionale**(con piggybacking): un host è sia mittente che destinatario
- **orientato al flusso di dati**: può trasmettere dati sottoforma di flusso: invia uno **stream di byte continuo**
- **orientato alla connessione**: viene effettuato un handshake prima di iniziare il flusso di dati
- **affidabile**(controllo degli errori)
- **controllo del flusso**
- **controllo della congestione**

>[!info] orientato al flusso di dati
possiamo immaginare la trasmissione con protocollo TCP come uno stream di byte continuo: immettiamo dati continuamente, e il protcollo li trasporta dall’altra parte (il TCP segmenta i dati, ma non è visibile a livello applicativo, i segmenti vengono gestiti a livello trasporto))
## struttura dei segmenti
socket source e socket destinazione

i numeri di sequenza si riferiscono all$i-esimo$ byte mandato. e viene mandato ack per ogni byte ricevuto correttamente (?)
### flag di controllo

| sigla | descrizione                                     |
| ----- | ----------------------------------------------- |
| `URG` |                                                 |
| `ACK` |                                                 |
| `PSH` | richiesta di push verso il livello applicazione |
| `RST` |                                                 |
| `SYN` |                                                 |
| `FIN` |                                                 |
## connessione TCP
3 fasi:
- apertura della connessione: 3 way handshake
- trasferimento dei dati
- chiusura della connessione

3 way handshake:
- mando byte con no dati, solo bit SYN ad 1, ed un numero random che sarebbe il numero di sequenza
- anche il server vuole aprire una connessione ! manda ack e SYN
- client manda ACK x il numero di sequenza inviato dal server

dati urgenti: URG flag
- bisogna anche guardare il puntatore urgente ( ci dice dove finiscono. vengono sempre messi all’inizio del segmento !)
- in questo modo quando il destinatario riceve dati urgenti, li deve passare subito a livello applicazione

x chiusura c’è un 3 way chiusura type beat
esiste anche un half close
## controllo degli errori
tipo selective repeat, 


- **checksum**:
- **riscontri + timer di ritrasmissione**(RTO):
- **ritrasmissione**:
### generazione di ack 
1. ack viene trasportato in piggybacking
2. aspetto 500ms perchè se mi arriva il primo segmento giusto, se nella rete non c’è congestione posso fare un ack cumulativo con un solo ack !
3. the good ending del primo caso. appena li ho entrambi mando un ack cumulativo (per entrambi)
4. segmento valido ma non ordinato: mando ack per il segmento prima che mi servee
dopo 3 ack di un segmento, lo ritrasmetto senza aspettare che finisca il timer (in modo veloce)



## controllo di flusso
il destinatario comunica al mittente lo spazio disponibile includendo 
**RWND**(receive window): permette al destinatario di comunicare al mittente quanto spazio ha all’interno della finestra di ricezione


l’ultimo ack è stato mandato xke viene allargata la window (ma non è utile)