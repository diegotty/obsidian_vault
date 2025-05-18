---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-18T13:15
completed: false
---
# reti wireless
le reti wireless si dividono in:
- **LAN wireless** (wifi)
- **reti cellulari**
- **bluetooth**
- **reti di sensori, RFID, smart objects**

>[!info] alcuni standard sulle reti wireless
![[Pasted image 20250518125106.png]]

quello più usato è lo standard IEEE 802.11, con le sue diverse versioni:

| Protocol | Release date | Freq.     | Rate (typical) | Rate (max) | Range (indoor) |
| -------- | ------------ | --------- | -------------- | ---------- | -------------- |
| Legacy   | 1997         | 2.4 GHz   | 1 Mbps         | 2 Mbps     | ?              |
| 802.11a  | 1999         | 5 GHz     | 25 Mbps        | 54 Mbps    | ~30 m          |
| 802.11b  | 1999         | 2.4 GHz   | 6.5 Mbps       | 11 Mbps    | ~30 m          |
| 802.11g  | 2003         | 2.4 GHz   | 25 Mbps        | 54 Mbps    | ~30 m          |
| 802.11n  | 2008         | 2.4/5 GHz | 200 Mbps       | 540 Mbps   | ~50 m          |
## LAN wireless
### elementi
>[!info] LAN wireless
![[Pasted image 20250518125705.png|400]]
gli elementi delle LAN wireless sono:
>- **wireless hosts**: utilizzati per eseguire applicazioni, possono essere stazionari o mobili, in quanto wireless non significa sempre mobile (es: laptops, cellulari)
>- **base stations**: sono **relay** (ripetitori) responsabili del mandare pacchetti tra network cablate e host wireless nell’area della base station (sono quindi tipicamente connessi ad un network cablato (es: cell towers, 802.11 access points)
>- **wireless links**: tipicamente usati per connetere dispositivi mobili alle base stations, possono anche essere usati come collegamento per il backbone. hanno vari range di rate e distanza di trasmissione, e il [[17 - livello di collegamento#protocolli di accesso multiplo|protocollo di accesso multiplo]] regola l’accesso al link

il mezzo trasmissione delle LAN wireless è l’aria, e la connessione ad altre reti avviene mediante una stazione base detta **access point** (**AP**) che **unisce l’ambiente wireless all’ambiente cablato**
>[!figure] access point
![[Pasted image 20250518130821.png]]

>[!tip] migrazione dall’ambiente cablato al wireless
il funzionamento di una rete cablato o wireless dipende dai due sottolivelli inferiori dello stack protocollare: collegamento e fisico
>
infatti, per migrare dalla rete cablata a quella wireless, è sufficiente cambiare le schede di rete e sostituire lo switch di collegamento con un **AP**
>- così facendo, cambiano gli indirizzi MAC mentre gli IP rimangono invariati

#### link wireless
vediamo alcune delle caratteristiche più importanti dei link wireless:
- **attenuazione del segnale**: la forza dei segnali elettromagnetici diminuisce rapidamente all’aumentare della distanza dal trasmettitore, in quanto il segnale si disperde in tutte le direzioni 
- **propagazione multi-path**: quando un’onda trova un ostacolo, tutta o parte dell’onda **viene riflessa** sull’ostacolo, rimbalzando con una perdita di potenza
	- un segnale sorgente, infatti , può arrivare tramite riflessi successivi (su muri, terreni, oggetti) a raggiungere una stazione/punto di accesso attraverso percorsi multipli ! (quindi ““ridondanza””, che in verità causa interferenze)
- **interferenze**: 
### reti ad hoc
le **reti ad hoc** non hanno hotspot o punti fissi, sono bensì un insieme di host che si auto-organizzano per formare una rete, e comunicano liberamente tra di loro
- ogni host deve eseguire funzionalità di rete, quali network setup, routing, forwarding, etc..
- viene usata per mettere in comunicazione più dispositivi noti 
## link wireless
vanno pensati per gestire le colliisioni

il raggio di trasmissione è quella distanza in cui segnale decodificabile

in caso di riflessi, il ricevitore riceve una omma di segnali che fanno interferenza tra di loro !
le interferenze