---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-12T23:31
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
**vantaggi**:
- in particolare, possiamo considerare un **modulo** come una black box (con oppurtuni ingressi e uscite), senza preoccuparci delle modalità con cui i dati vengono elaborati per avere un output. in questo modo **un livello usa servizi dal livello inferiore e offe servizi al livello superiore**, indipendentemente da come sia implementato
	- in questo modo, è inoltre possibile modificare un modulo in modo trasparente, se le interfacce con gli altri livelli rimangono le stesse
- possiamo gestire tecnologie diverse (wireless, con fili) senza dover reimplementare tutto
- inoltre, se due macchine forniscono lo stesso output dato il medesimo input, possono essere considerate equivalenti e possono quindi essere acquistate da fornitori diversi (se equivalenti)
	- e ciascun costruttre può adottare la proprima implementazione di un livello, purchè soddisfi i requisiti sulle interfacce
**svantaggi**:
- a volte può essere necessario lo scambio di informazioni tra livelli non adiacenti, non rispettando quindi il principio della stratificazione
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

>[!warning] servizi e protocolli sono concetti ben distinti !
> - un servizio è un insieme di primitive che uno strato offre a quello superiore (non dice nulla su come sono implementate le operazioni che offre il livello)
>- un protocollo è un insieme di regole che controllano il formato e il significato dei pacchetti, o messaggi scambiati tra le entità pari all’interno di uno strato
![[Pasted image 20250312224953.png]]

>[!info] esempio più dettagliato
![[Pasted image 20250312225518.png]]
>- **interfacce**: definiscono le operazioni elementari e i servizi che lo strato inferiore rende disponibili a quello soprastante
>- i processi di ogni stato (o almeno quelli più alti), modellano la loro comunicazione come se fosse “orizzontale”: avranno funzioni del tipo `sendToOtherSide`, `getFromOtherSide` (quindi non si preoccupano del resto dello stack sottostante, ma usano i serivizi dei livelli inferiori). queste procedure di alto livello comunicano, in verità, mediante strati inferiori
>- la comunicazione virtuale, cioè la comunicazione modellata da ogni strato, è disegnata in rosso
>- la comunicazione reale, che attraversa quindi ogni livello dello stack protocollare, è disegnata in blu

## incapsulamento e decapsulamento
durante la comunicazione, l’host mittente effettua **l’incapsulamento**: prende il pacchetto dal livello superiore, lo considera come payload e aggiunge un header.
l’host destinatario invece effettua il decapsulamento
>[!tip] il router effettua sia incapsulamento che decapsulamento
>perchè è collegato a due link (??) quindi decapsula quando riceve il pacchetto, per poi incapsularlo (presumibilmente con informazioni diverse) e mandarlo al prossimo link

in particolare, i pacchetti cambiano nome ad ogni livello in cui si trovano (in quanto vengono aggiunge informazioni ad ogni livello). abbiamo:
- livello 5: messaggio (nessun header)
- livello 4: segmento/datagramma utente (aggiunta di header a livello di trasporto)
- livello 3: datagramma (aggiunta di header a livello di rete)
- livello 2: frame (aggiunta di header a livello di collegamento)
- livello 1: bit (roba seria)
>[!example]- esempio
![[Pasted image 20250312230642.png]]

### multiplexing e demultiplexing
dato che lo stack TCP/IP prevede più protocolli nello stesso livello, è necessario:
- eseguire **multiplexing** alla sorgente: un protocollo può dover incapsulare, uno alla volta, pacchetti ottenuti da più protocolli del livello superiore
- eseguire **demultiplexing** alla destinazione: un protocollo può dover decapsulare e consegnare pacchetti ottenuti da protocolli diversi dal livello inferiore, da consegnare al livello superiore
>[!info] è quindi necessario un campo nel proprio header per identificare a quale protocollo appartengano i pacchetti incapsulati
![[Pasted image 20250312231210.png]]

>[!figure] img
![[Pasted image 20250312231119.png]]

## indirizzamento nel modello TCP/IP
dato che il modello TCP/IP prevede una comunicazione logica tra coppie di livelli, è necessario avere un indirizzo sorgente ed un indirizzo destinazione ad ogni livello (ad ogni livello, devo dire a chi devo mandarlo: devo usare un indirizzo del livello pari ! in quanto, per modularità, i livelli sono indipendenti e la comunicazione è logica)
>[!info] immagine riassuntiva
![[Pasted image 20250312231514.png]]

# modello OSI
l’ISO ha definito il **modello OSI** come modello alternativo al TCP/IP, ma non si è diffuso, in quanto alla sua pubblicazione, il TCP/IP era già ampiamente diffuso, e non riuscirono a dimostare delle prestazioni tali da convincere le autorità di Internet a sostituire il TCP/IP
>[!info] confronto tra OSI e TCP/IP
![[Pasted image 20250312232356.png]]

>[!info] Internet lore
# Internet standard
nello studio di Internet e dei suoi protocolli, si incontrano spesso riferimenti a standard o entità amministrative: uno **standard Internet** è una specifica che è stata rigorosamente esaminata e controllata, ritenuta utile ed accettata da chi utilizza la rete Internet. è quindi un insieme di regole formalizzate, che devono essere necessariamente seguite
>[!in]

la velocità che ci permette un protocollo non affidabile può esere un buon compromesso per usarlo (purchè non perdiamo troppi dati)