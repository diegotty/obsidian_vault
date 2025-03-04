---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-04T22:09
completed: false
---
>[!info] scopo del corso: capire cosa succede all’interno della rete
>- imparare come funziona internet
>- acquisire i concetti fondamentali relativi alle reti di elaboratori: livelli, servizi, protocolli
>- conoscere le problematiche fondamentali,e le relative soluzioni, incontrate nella definizione dello stack protocollare di una moderna architettura di rete (affidabilità, sicurezza, etc)
>- conoscere caratteristiche e funzionamento dei vari livelli e protocolli dell’architettura di rete **TCP/IP**
# le reti
**pacchetti**: blocchi di dati che viaggiano in rete

>[!info] rete  
>![[Pasted image 20250302183128.png]]
rete è composta da dispostivi in grado di **scambiarsi informazioni** (end system), e dispositivi di interconessione (che permettono di far viaggiare le informazioni)
![[Pasted image 20250304214229.png]]
## dispositivi terminali
i sistemi terminali possono essere di 2 tipi:
### host
macchine in genere di proprietà degli utenti e dedicate ad eseguire operazioni
- computer desktop, portatili, cellulari, tablet, …
### server
tipicamente, computer con elevate prestazioni destinati a eseguire programmi che forniscono servizi a diverse applicazioni utente come, per esempio, la posta elettronica o il Web
- vengono gestiti da amministratori di sistema
## dispositivi di interconnessione
i dispositivi di interconnessione **rigenerano**/modificano il segnale che ricevono e si distinguono in:
 - **router**: dispositivi che collegano una rete ad altre reti
	- usati nel backbone internet
- **switch**: o commutatori, collegano più sistemi terminali (**instradano**) a livello locale
- **modem**: **trasformano** la codifica dei dati (trasformano i dati digitali del computer in dati analogici, e viceversa)
## collegamenti
i dispositivi di rete vengono collegati utilizzando mezzi trasmissivi cablati o wireless, genericamente chiamati **link** 
### mezzi trasmissivi cablati
i dati viaggiano da un sistema terminale ad un altro, passando per una serie di coppie trasmittente-ricevente, attraverso mezzi fisici
sempi di mezzi fisici: doppino intrecciato, cavo ethernet,
- cavo coassiale: 2 conduttori concentrici in rame, usato per cablatura di reti locali ad alta velocità.
- fibra ottica: conduce impulsi di luce, in cui ciascun impulso rappresenta un bit. ad alta frequenza trasmissiva (da 10 a 100 Gbps), e basso tasso di errore (anche immune all’interferenza elettromagnetica). è il mezzo prevalente delle dorsali di internet

>[!warning] i nodi sono trasmittente E ricevente alllo stesso tempo 
>si parla di nodo trasmittente/ricevente solo per indicare la direzione della commutazione


### mezzi trasmissivi wireless
i segnali si propagano nell’atmosfera e nello spazio esterno
- soffrono dell’ambiente circostante
- propagandosi in tutte le direzioni, si riflettono quando trovano degli ostacoli
- ci sono dei materiali che ostruiscono il passaggio 
- ci possono essere interferenze da altre tecnologie

# classificazione delle reti

| scale    | type                                   | example        |
| -------- | -------------------------------------- | -------------- |
| vicinity | PAN(personal area network)             | bluetooth      |
| building | LAN(local area network)                | wifi, ethernet |
| city     | MAN(metropolitan area network)         | cable, DSL     |
| country  | WAN(wide area network)                 | large ISP      |
| planet   | the Internet (network of all networks) | the Internet ! |

## reti LAN
solitamente, è una rete privata che collega sistemi terminali in un singolo ufficio (azienda, università)
- ogni sistema terminale ha un indirizzo univoco che lo identifica nella rete
>[!info] esempio di LAN con cavo condiviso (rete broadcast)
![[Pasted image 20250304220823.png]]
obsoleto !!!
>- il pacchetto inviato da un dispositivo viene ricevuto da tutti gli altri (broadcast)
>- solo il destinatario elabora il pacchetto, tutti gli altri lo ignorano
>- può trasmettere un nodo alla volta

>[!info] esempio d LAN con switch di interconnessione (topologia a stella)
![[Pasted image 20250304220948.png]]
lo switch gestisce a chi inviare i pacchetti che gli arrivano 
lo swtich è in grado di trasmettere su più porte, quindi gli host possono comunicare parallelamente ()

## reti WAN
interconnette switch, router, e modem
- gestita da un ISP (internet service provider)
-WAN punto-punto
-WAN a commutazione (usata nelle dorsali di internet)

WAN punto-punto per avere una rete preivata (slide 25)

### la rete GARR
interconnette ad altissima capacità 

# switching
o commutazione
router: switch di livello 3
switch: switch di livello 2
ci sono 2 tipi di reti basate su switch:
## reti a commutazione di circuito
viene decisono un percorso: il circuito ( aka vengono riservate le risorse necessarie per la comunicazione : la banda full-link, le risorse fisiche presenti sugli switch)
	- ci garantisce, che una volta stabilito un circuito, c’è una certa capacità garantita, 
	- banda: capacità di trasmissione: quanti bit posso trasmettere in un unità di tempo
	- slide 32: tutti i dispositivi possono comunicare allo stesso tempo, a ogni dispositivo viene dato 1/4 della banda (quindi potrebbe essere sottuitilizzata)
suddividere la banda: 
- **FDM**
- **TDM**

## reti a commutazione di pacchetto (store and forward)
modo in cui funziona la rete internet oggi 
utente spedice i pacchetti al router, che li riceve tutti. li mette in una coda (canale seriale), e spedisce i pacchetti al prossimo router
router hanno code in ingresso e code in uscita (nell’esempio i router hanno 5 code in ingresso e 5 code in uscita)

un percorso si può sovraccaricare, e i router si posssono congestionare. a quel punto può essere utile cambiare percorso per inviare il pacchetto ( le code del router congestionato saranno piene, e i pacchetti che arrivano vengono scartati)

analogia con sistema postale perchè le cose viaggiano indipendentemente e non sai se due cose mandate allo stesso tempo allo stesso destinatario arriveranno insieme o molto distanti

molto più flessibile in termini di prestazioni: se io sono solo posso utilizzare la rete al massimo,se siamo in 2 a metà, etc

## rappresentazione concettuali di Internet
### ARPANET

rete di accesso a internet: 
## via rete telefonica
limitazione: velocità, non si possono inviare contemporaneamente dati e voce (L) (dsl fixa ciò)
dsl: un filtro che prende sia dati voce che dati dal modem

- tramite reti wirless ( + utilizzato )
- collegamento diretto