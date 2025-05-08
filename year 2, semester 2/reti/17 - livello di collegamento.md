---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-08T14:44
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

>[!info] errori su singolo bit o a burst

il livello di collegamento è implementato all’interno della **network interface card** (**adattatore di rete**: la scheda ethernet, PCMCI)

il livello di collegamento può essere separato in due sottolivelli:
## data-link control
## media access control