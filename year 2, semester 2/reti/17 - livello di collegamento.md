---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-08T14:39
completed: false
---
i protocolli a livello collegamento si occupano del traporto di datagrammi lungo un canale di comunicazione (fisico o wireless)
### tipi di collegamento
- **collegamento punto-punto**: dedicato a due soli dispositivi
- **collegamento broadcast**: il collegamento è condivisio tra varie coppie di dispositivi
## servizi offerti dal livello di collegamento
- **framing**: i protocolli incapsuano i datagrammi del livello di rete all’interno di un frame a livello di link
- **consegna affidabile**: la trasmissione è basata su `ACK` come nel trasporto
- **controllo di flusso**
- **rilevazione degli errori**: gli errori sono causati dalle **interferenze**: rumore elettromagnetico e ““sovrapposizione” del segnale
- **correzione degli errori**:

il livello di collegamento è implementato all’interno dell’**adattatore di rete**