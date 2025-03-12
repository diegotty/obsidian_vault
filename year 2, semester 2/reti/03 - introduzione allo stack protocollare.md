---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-12T20:01
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
# stack protocollare TCP/IP
**TCP/IP** è una famiglia di protocolli attualmente utilizzata in Internet, costruita come una **gerarchia di protocolli** (ogni livello si basa sui servizi forniti dai livelli inferiori) costituita da moduli interagenti, ciascuno dei quali fornisce funzionalità specifiche
- si chiama così perchè TCP e IP sono i 2 protocolli principali dello stack
>[!info] stack TCP/IP
![[Pasted image 20250312193517.png]]
tatoo it on your forhead

## livello 5: applicazione
è il livello in cui si interagisce con le applicazioni di rete, attraverso vari protocolli che regolano la comunicazione (tra utente e applicazione ?): 
- HTTP, SMTP, FTP, DNS, ….
## livello 4: trasporto
è il livello in cui vengono trasferiti i messaggi a livello di appplicazone, da un client ad un server. si può scegliere tra i protocolli:
- TCP, UDP
## livello 3: rete
è il livello che si occupa dell’instradamento dei segmenti dall’origine alla destinazione (rotte end-to-end). fa si che i pacchetti seguano il percorso corretto per arrivare a destinazione !
## livello 2: link
è il livello che si occupa della trasmissione dei pacchetti nella singola tratta (hop to hop): da un nodo a quello successivo sul percorso. 
## livello 1: fisico
è il livello che gestisce il trasferimento dei singoli bit su lungo un canale di comunicazione
>[!info] comunicazione in una internet
![[Pasted image 20250312195732.png]]
>- grazie al layering, i sistemi implementano solo i livelli necessari, riducendo la complessità !
>- nel router, ci possono essere fino a $n$ livelli fisico e livello collegamento, dove $n$ è il numero di link a cui è collgato (questo perchè possono avere protocolli di collegamento diversi)
>- invece essendo le porte dello swtich omogenee, c’è solo un protocollo (una tecnologia )

la velocità che ci permette un protocollo non affidabile può esere un buon compromesso per usarlo (purchè non perdiamo troppi dati)

il pacchetto ha un nome diverso ad ogni livello perchè ad ogni livello aggiungo infomazioni


con il layering possiamo gestire tecnologie diverse (wireless, con fili) senza dover reimplementare tutto

il modello OSI non si è diffuso. non ha messo abbastanza tiktok virali


# modello TCP/IP


	
	