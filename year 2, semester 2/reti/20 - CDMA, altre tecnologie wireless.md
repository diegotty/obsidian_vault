---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-23T10:37
completed: false
---
abbiamo visto, nelle lezioni precedenti, i vari modi di accesso al mezzo mediante suddivisione del canale: [[17 - livello di collegamento#TDMA|TDMA]] e [[17 - livello di collegamento#FDMA|FDMA]]
studiamo ora un atro protocollo per accesso multiplo
## CDMA
nel **CDMA** (**code division multiple access**):
- l’intera ampiezza di banda viene occupata da un solo canale (non c’è quindi divisone di frequenze)
- tutte le stazioni possono invare contemporaneamente 
- vengono usati **codici**, diversi per ogni stazione, per comunicare in modo “riconoscibile” (come se ogni coppia di stazioni parlasse una lingua diversa)
>[!example] esempio
>assumiamo di avere 4 stazioni connesse sullo stesso canale:
>- i dati spediti sono $d_{1},d_{2},d_{3},d_{4}$, e i codici assegnati alle stazioni sono $c_{1},c_{2},c_{3},c_4$
>
![[Pasted image 20250523103239.png]]

nel CDMA, ogni stazione **moltiplica** i propri dati per il proprio codice, e trasmette il risultato
>[!info] proprietà dei codici
>- se moltiplichiamo ogni codice con un altro codice, otteniamo 0
>- se moltiplichiamo ogni codice per se stesso, otteniamo il numero delle stazioni (vedremo che ogni digit del codice è in $\{ 1,-1\}$)
>- **qualsiasi stazione voglia ricevere dati da una delle altre tre stazioni (nell’esempio), moltiplica i dati ricevuti per il codice del mittente e divide per il numero delle stazioni**

>[!example] esempio di calcoli
consid

un codice moltiplicato per se stesso da il numero di stazioni

sommo tutto  e lo mando sul canale
spreading: far diventare un bit una sequenza



ogni slot lavora da una sequenza differente (slide 26 )

su una sottorete, le piconet lavorano a frequenze diverse

se due tag rispondono nello stesso momento, avviene una collisione (praticametne bit a bit) e il reader rimane incluato


reader deve arbitrare il protcoollo MAC


tree slotted aloha 42% di eff.