---
related to: "[[03 - introduzione allo stack protocollare TCP-IP]]"
created: 2025-03-02T17:41
updated: 2025-03-21T11:41
completed: false
---
# livello trasporto
i protocolli di trasporto forniscono la **comunicazione logica** tra processi applicativi di host differenti
- gli host eseguono i processi come se fossero direttamente connessi, anche se sono agli antipodi del pianeta (a loro non frega nulla !!!)
i protocolli di trasporto vengono quindi eseguiti nei **sistemi terminali**:
- **lato invio**: **incapsula** i messaggi in **segmenti** e li passa al ivello di rete
- **lato ricezione**: **decapsula** i segmenti in messaggi e li passa al livello applicazione
>[!info] livello di trasporto e livello di rete
il livello di trasporto quindi comunica con il livello di rete, ma offronto servizi diversi:
>- **livello di trasporto**: si occupa della comunicazione tra processi (si basa sui servizi del livello reti e li potenzia)
>- **livello di rete**: si occupa della comunicazione tra host (si basa sui servizi del livello collegamento)
![[Pasted image 20250321110954.png]]
>>[!example] esempio
>>**analogia con la posta ordinaria**:
>>una persona di un condominio invia una lettera ad una persona di un altro condominio consegnandola/ricevendola da/a un portiere
>>- processi = persone
>>- messaggi delle applicazioni = lettere
>>- host = comdomini
>>- protocollo di trasporto = portieri dei condomini ( i portieri svolgono il proprio lavoro localmente, non sono coinvolti nella tappe intermedie delle lettere, così come il protocollo di trasporto)
>>- protocollo del livello di rete = servizio postale
## indirizzamento
la maggior parte dei SO è **multiutente** e **multiprocesso**, quindi ci sono:
- diversi processi clienti attivi (host locale)
- diversi processi server attivi (host remoto)
per stabilire una comunicazione tra i due processi è necessario un metodo per individuare:
- **host locale** → indirizzo IP locale
- **host remoto** → indirizzo IP remoto
- **processo locale** → numero di porta
- **processo remoto** → numero di porta
un **socket address** è l’unione di indirizzo IP e porta
### incapsulamento/decapsulamento
>[!info] illustrazione !
![[Pasted image 20250321112311.png]]
>i pacchetti a livello di trasporto sono chiamati **segmenti** se viene usato il protocollo TCP, o **datagrammi utente** se viene usato il protocollo UDP
### multiplexing/demultiplexing
permette di rendere il servizio di trasporto da **host a host** fornito dal livello di rete un servizio di trasporto da **processo a processo** per le applicazioni eseguite sugli host
- **multiplexing dell’host mittente**: raccogliere i dati da varie socket, incapsularli con l’intestazione e passarli al livello di rete
- **demultiplexing dell’host destinatario**: consegnare i segmenti ricevuti alla socket appropriata. una volta ricevuti i dati del livello di rete, indirizza i dati a uno dei processi dell’'host ricevente grazie alle informazioni all’interno dell’header
#### demultiplexing
l’host riceve i datagrammi IP (pacchetti del livello rete)
- ogni datagramma ha un indirizzo IP di origine e un indirizzo IP di destinazione, e trasporta un segmento a livello di trasporto:
	- il segmento ha un numero di porta di origine e un numero di porta di destinazione
>[!info]- struttura del pacchetto TCP/UDP
![[Pasted image 20250321113540.png]]

l’host usa quindi indirizzi IP e numeri di porta per inviare il segmento al processo appropriato
## API di comunicazione
>[!info] huh
![[Pasted image 20250321113643.png]]

il **socket** 

### numeri di porta
i numeri di porta sono indirizzi di 16bit, che vanno quindi da 0 a 65535, divisi in:
- **well known ports**: porte registrate per server comuni
	- FTP 20
	- TELNET 23
	- SMTP 25
	- HTTP 80
	- POP3 110
	- ….
- **assigned ports**
	- 0 non usata
	- 1-255 riservate per 