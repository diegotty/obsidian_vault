---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-28T20:10
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
## segmenti TCP
>[!info] illustrazione
![[Pasted image 20250328194753.png]]
>- TCP riceve uno stream di byte dal processo mittente (livello applicazione)
>- TCP utilizza il servizio di comunicazone tra host del livello di rete che invia i pacchetti: TCP deve raggruppare un certo numero di byte in **segmenti**, aggiungere un’intestazione e consegnare al livello di rete per la trasmissione
### struttura dei segmenti
>[!info] struttura dei segmenti !
![[Pasted image 20250328195117.png]]
>le informazioni mandatore di un header occupano quindi 20 byte.
in particolare:
>- il **numero di sequenza** indica il numero del primo byte di dati contenuto nel segmento
>- l’`ACK` (**numero di riscontro**) indica il numero di sequenza del prossimo byte che il destinatario si aspetta di ricevere

i numeri di sequenza si riferiscono quindi all’$i-esimo$ byte inviato.
#### flag di controllo

| sigla | descrizione                                                          |
| ----- | -------------------------------------------------------------------- |
| `URG` | contiente messaggio urgente (puntatore urgente valido)               |
| `ACK` | riscontro valido                                                     |
| `PSH` | richiesta di push verso il livello applicazione                      |
| `RST` | azzeramento della connessione                                        |
| `SYN` | richiesta di connessione (e sincronizzazione dei numeri di sequenza) |
| `FIN` | chiusura della connessione                                           |
## connessione TCP
la connessione TCP è composta da 3 fasi:
- apertura della connessione
- trasferimento dei dati
- chiusura della connessione
### apertura della connessione
viene effettuato un **3 way handshake**:
>[!info] 3 way handshake
![[Pasted image 20250328195908.png]]
>- il client manda un pacchetto composto da: `SYN=1`, `seq`: un numero generato**randomicamente** che diventerà il numero di sequenza durante il trasferimento
>- il server risponde con un pacchetto composto da: `ACK` del pacchetto ricevuto, `SYN=1`(in quanto anche il server vuole aprire una connessione con il client), `rwnd` con la dimensione della finestra di ricezione (spiegato più avanti)
>- il client risponde con un `ACK` del pacchetto ricevuto, risponde con la sua `rwnd`. la connessione è aperta da entrambi i lati, e può iniziare il trasferimento di dati

### trasferimento dati
>[!info] trasferimento dati: push
>[!info] trasferimento dati : urgent
>può capitare che ci sia la necessità di mandare dati **urgenti**, cioè da far arrivare al livello applicazione il prima possibile, senza aspettare il `push`. in questo caso, il flag `URG` sarà valido (il suo valore sarà 1). ciò indica che bisogna controllare il **puntatore urgente**:
>- i dati urgenti vengono inseriti all’inizio di un nuovo segmento, che può contenere dati non urgenti a seguire. il puntatore urgente indica dove i dati urgenti finiscono

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