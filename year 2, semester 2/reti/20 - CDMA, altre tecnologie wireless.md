---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-23T10:52
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
consideriamo l’esempio di sopra:
>$$
\text{messaggio sul canale = }d_{1}\cdot c_{1}+d_{2}\cdot c_{2}+d_{4}\cdot d_{4}
>$$
>$$
\text{messaggio di stazione1} = \frac{(d_{1}\cdot c_{1}+d_{2}\cdot c_{2}+d_{4}\cdot d_{4})\cdot c_{1}}{4} =
>$$
>$$
\frac{\Big((d_{1}\cdot c_{1})\cdot c_{1}+(d_{2}\cdot c_{2})\cdot c_{1}+(d_{4}\cdot d_{4})\cdot c_{1}\Big)}{4} = \frac{d_{1}\cdot4 + d_{2}\cdot0+ d_{4}\cdot0}{4} = d_{1}
>$$

## sequenze di chip
il CDMA si basa sulla teoria della codifica: ad ogni stazione viene assegnato un codice (sequenza di numeri), chiamati chip
- le sequenze assegnate sono speciali e vengono chiamate **sequenze ortogonali**
>[!info] proprietà delle sequenze ortogonali
> - ogni sequenza è composta da $N$ elementi, con $N= \text{numero di stazioni}$ 
> 	- $N$ deve essere una potenza di 2 (per come calcoliamo le sequenze !)
>- se moltiplichiamo una sequenza per un numero, ogni elemento della sequenza viene moltiplicato per tale numero
>	- $2\cdot[+1+1-1-1]= [+2+2-2-2]$
> se moltiplichamo due sequenze uguali e sommiamo i risultati otteniamo $N$
> 	- $[+1+1-1-1]\cdot[+1+1]$


sommo tutto  e lo mando sul canale
spreading: far diventare un bit una sequenza



ogni slot lavora da una sequenza differente (slide 26 )

su una sottorete, le piconet lavorano a frequenze diverse

se due tag rispondono nello stesso momento, avviene una collisione (praticametne bit a bit) e il reader rimane incluato


reader deve arbitrare il protcoollo MAC


tree slotted aloha 42% di eff.