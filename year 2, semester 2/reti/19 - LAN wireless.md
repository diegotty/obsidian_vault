---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-18T14:50
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

### reti ad hoc
le **reti ad hoc** non hanno hotspot o punti fissi, sono bensì un insieme di host che si auto-organizzano per formare una rete, e comunicano liberamente tra di loro
- ogni host deve eseguire funzionalità di rete, quali network setup, routing, forwarding, etc..
- viene usata per mettere in comunicazione più dispositivi noti 
## link wireless
vediamo alcune delle caratteristiche più importanti dei link wireless:
- **attenuazione del segnale**: la forza dei segnali elettromagnetici diminuisce rapidamente all’aumentare della distanza dal trasmettitore, in quanto il segnale si disperde in tutte le direzioni 
	- il **raggio di trasmissione** è infatti la distanza in cui un segnale è ancora decodificabile da un host
- **propagazione multi-path**: quando un’onda trova un ostacolo, tutta o parte dell’onda **viene riflessa** sull’ostacolo, rimbalzando con una perdita di potenza
	- un segnale sorgente, infatti , può arrivare tramite riflessi successivi (su muri, terreni, oggetti) a raggiungere una stazione/punto di accesso attraverso percorsi multipli ! (quindi ““ridondanza””, che in verità causa interferenze, in quanto l’host riceve una somma di segnali che creano interferenze)
- **interferenze**: possono essere causate dalla stessa sogente (propagazione multi-path), o da altre sorgenti (altri trasmettitori usano la stessa banda di frequenza per comunicare con altri destinatari)
### errori
le caratteristiche dei link wireless causano errori
il **signal to noise ratio** (**SNR**) misura il rapporto tra il segnale buono e quello “cattivo”
- alto: il segnale è più forte del rumore, quindi può essere convertito in dati reali
- basso: il segnale è stato danneggiato dal rumore, e i dati non possono essere recuperati
inoltre, dato che i link wireless sono un mezzo condiviso, è necessario controllare l’accesso ad essi per evitare collisioni 
>[!info] CSMA/CD
abbiamo visto che con la trasmissione ethernet si usa il protocollo di accesso multiplo CSMA/CD. è possibile usarlo con sulle reti wireless ?
**no!** in quanto per rilevare una connessione, un host deve poter trasmettere il proprio frame e ascoltare, contemporaneamente, il canale. ma, poichè la potenza del segnale ricevuto è molto inferiore a quella del segnale trasmesso, sarebbe troppo costoso (soprattutto a livello di energia, in quanto i dispositivi wireless usano spesso una batteria) usare un adattatore di rete in grado di rilevare le collisioni 
>inoltre, un host potrebbe non accorgersi che un altro host sta trasmettendo, e quindi non sarebbe in grado di rilevare la connessione pur ascoltando il canale
> - chiamato anche hidden terminal problem !
>
![[Pasted image 20250518133359.png]]
>
>quindi non si può contare sulla collision detection nelle reti wireless !
## IEEE 802.11
IEEE ha definito le specifiche per le LAN wireless, chiamate 802.11, che coprono i livelli fisico e collegamento
### architettura
>[!info] architettura generale
![[Pasted image 20250518134144.png]]
#### BSS
**basic service set** (**BSS**) è costituita da uno o più host wireless e da un access point (che è collegato ad un router)
- corrispondono alle **celle** delle reti cellulari
>[!info] BSS
![[Pasted image 20250518133830.png]]
>- il BSS con infrastruttura è l’architettura più diffusa !
>- il BSS ad hoc è invece una rete *standalone*
#### ESS
**extended service set** (**ESS**) è costituito da due o più **BSS** con infrastruttura, collegati tramite un sistema di distribuzione che è una rete cablata (ethernet) o wireless
- quando i BSS sono colleagti, le stazioni in visibilità comunicano direttamente mentre le altre comunicano tramite l’AP
- molto comuni nelle reti wi-fi moderne, sopratutto in ambienti dove è necessario coprire aree estese con accesso continuo alla rete wireless
>[!info] ESS
![[Pasted image 20250518134043.png]]
## canali e associazione
lo spettro 2.4GHz-2.48GHz è diviso in 11 canali parzialmente sovrapposti:
- l’amministratore dell’AP sceglie una frequenza su cui far comunicare l’AP
- sono possibili interferenze (es: se viene usato lo stesso canale per AP vicini)
- il numero massimo di frequenze utilizzabili da diversi AP per evitare interferenze è 3
- i canali non interferiscono se separati da 4 o più canali
dato che architettura IEE 802.11 prevede che una stazione wireless si associ a un AP per accedere a internet, è necessario:
- conoscere gli AP disponibili in un BSS
- un protocollo di associazione:
	- AP invia segnali periodici, chiamati **beacon**, che includono l’identificatore dell’AP: il **SSID** (**service set identifier**), e il suo indirizzo MAC
	- l’host wireless che vuole entrare in un BSS scandisce gli 11 canali trasmissivi alla riceva di frame beacon (passive scanning)
	- alla fine della scansione, l’host sceglie l’AP da cui ha ricevuto il beacon con maggiore potenza di segnale, e gli invia un frame con la richiesta di associazione
	- l’AP accetta la richiesta con un frame di risposta associazione, che permetterà all’host entrante di inviare una richiesta DHCP per ottenere l’indirizzo IP
		- può essere prevista un’autenticazione per eseguire l’associazione
## accesso multiplo
più stazioni possono voler comunicare nello stesso momento, e per gestire ciò esisono 2 tecniche di accesso al mezzo:
- **distributed cordination function** (**DCF**): i nodi si contendono l’accesso al canale
- **point coordination function** (**PCF**): non c’è contesa, e l’AP coordina l’accesso dei nodi al canale (boooooringgggg)
vediamo il DCF:
### CSMA/CA
dato che non è possibile contare sulla **collision detection**, obblighiamo la **collision avoidance** (da cui CSMA/**CA**): evitiamo che due o più nodi trasmettano simultaneamente
il protocollo **CSMA/CA** usa gli `ACK` come riscontro necessario per capire se una trasmissione è andata a buon fine (no collisione)