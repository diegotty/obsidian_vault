---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-28T14:52
completed: false
---
**protocollo con pipeline**:
**bidirezionale**(con piggybacking): 
**orientato al flusso di dati**: può trasmettere dati sottoforma di flusso 
**orientato alla connessione**:
**affidabile**(controllo degli errori):
**controllo del flusso**:
**controllo della congestione**:

orientato al flusso di dati
invia uno stream di byte continuo (immettiamo dati continuamente, il protcollo li trasporta dall’altra parte (il tcp segmenta i dati, ma non è visibile a livello applicativo, gestito tutto a livello trasporto))
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
2. aspetto 500ms perchè se mi arriva il primo segmento giusto, se nella rete non c’è congestione posso fare un ack cumulativo con un solo ack !
3. the good ending del primo caso. appena li ho entrambi mando un ack cumulativo (per entrambi)
4. segmento valido ma non ordinato: mando ack per il segmento prima che mi servee
dopo 3 ack di un segmento, lo ritrasmetto senza aspettare che finisca il timer (in modo veloce)

