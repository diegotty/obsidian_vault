---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-12T19:31
completed: false
---
# introduzione allo stack protocollare

abbiamo una panoriamica della struttura e delle varie prestazioni di Internet: sappiamo che è costituita da numerose reti di varie dimensioni, interconnesse tramite opportuni dispositivi di comunicazione. tuttavia, per poter comunicare non è sufficiente assicurare questi collegamenti, ma è necessario utilizzare sia l’hardware che del software !
>[!example]- esempio di comunicazione
![[Pasted image 20250312191705.png]]
>due interlocutori rispettano un protocollo di conversazione:
>- si inizia con un saluto
>- si adotta un linguaggio appropriato al livello di conoscenza
>- si tace mentre l’altro parla
>- la conversazione si sviluppa come un dialogo piuttosto che un monologo
>- si termina con un saluto
## protocollo
un **protocollo** definisce le regole che il mittente, destinatario e tutti i sistemi intermeti coinvolti, devono rispettare per poter comunicare
- potrebbe bastare un solo protocollo, o potrebbe essere necessario dividere i compiti in livelli, dove in ogni livello è richiesto un protocollo: si parla di **layering di protocolli**
## strutturazione a livelli
oltre a consentire la suddivisione di un compito complesso in più compiti semplici, la strutturazione a livelli permette di avere livelli indipendenti tra loro (**modularizzazione**).
- in particolare, possiamo considerare un **modulo** come una black box (con oppurtuni ingressi e uscite), senza preoccuparci delle modalità con cui i dati vengono elaborati per avere un output. in questo modo **un livello usa servizi dal livello inferiore e offe servizi al livello superiore**, indipendentemente da come sia implementato
- inoltre, se due macchine forniscono lo stesso output dato il medesimo input, possono essere considerate equivalenti e possono quindi essere acquistate da fornitori diversi (se equivalenti)
## stack protocollare TCP/IP

slide 9

TCP e IP: 2 protocolli principali di tutto lo stack
- gerarchia di protocolli

web: applicazione. http: protocollo che regola la comunicazione

trasporto> trasferisce i messaggi da un client ad un server

rete: trova le rotte(end-to-end), fa si che i pacchetti (datagrammi) seguano il percorso corretto per arrivare a destinazione 

link: si occupa della trasmissione della singola tratta (hop to hop)

fisico: trasferimento dei singoli bit su lungo un canale di comunicazione

essendo le porte dello swtich omogenee, c’è solo un protocollo (una tecnologia )

la velocità che ci permette un protocollo non affidabile può esere un buon compromesso per usarlo (purchè non perdiamo troppi dati)

il pacchetto ha un nome diverso ad ogni livello perchè ad ogni livello aggiungo infomazioni


con il layering possiamo gestire tecnologie diverse (wireless, con fili) senza dover reimplementare tutto

il modello OSI non si è diffuso. non ha messo abbastanza tiktok virali


# modello TCP/IP


	
	