---
related to: "[[03 - introduzione allo stack protocollare TCP-IP]]"
created: 2025-03-02T17:41
updated: 2025-03-21T11:56
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

il **socket** è un’astrazione (?): una struttura dati creata e utilizzata dal programma applicativo, che permette di comunicare, attraverso il socket,  tra un processo client e un processo server (???????)
>[!info] illustrazione
![[Pasted image 20250321114427.png]]
### numeri di porta
i numeri di porta sono indirizzi di 16bit, che vanno quindi da 0 a 65535, divisi in:
- **well known ports**: porte riservate per server comuni e noti
	- FTP 20
	- TELNET 23
	- SMTP 25
	- HTTP 80
	- POP3 110
	- ….
- **assigned ports**
	- 0: non usata
	- 1-255: **well known ports**
	- 256-1023: riservate per altri processi
	- 1024-65535: dedicate a user apps (utilizzabili da tutti per nuove applicazioni)

dato che l’interazione tra client e server è bidirezionale, è necessaria una coppia di indirizzi socket: **locale**(mittente) e **remoto**(destinatario)
- l’indirizzo locale in una direzione è l’indirizzo remoto nell’altra direzione !
### individuare i socket
come vengono individuati questi indirizzi ?
>[!info] individuare i socket lato client
>- **socket adress locale**: fornito dal SO, in quanto conosce l’indirizzo ip del computer su cui il client è in esecuzione. il numero di porta è assegnato temporaneamente dal SO 
>- **socket address remoto**: il numero di porta è conosciuto in base all’applicazione, mentre l’indirizzo IP è fornito dal DNS 

>[!info] individuare i socket lato server
>- **socket address locale**: fornito dal SO, in quanto conosce l’indirizzo IP del computer su cui il server è in esecuzione. il numero di porta è noto al server perchè assegnato dal progettista ! (numero well known o scelto)
>- **socket address remoto**: è il socket address locale del client che si connette ! lo trova quindi all’interno del pacchetto di richiesta
>[!warning] il socket address locale di un server non cambia(rimane fisso e invariato), mentre il socket address remoto varia ad ogni interazione con client diversi ( o stesso client su connessioni diverse)
# servizi di trasporto
## overview
**TCP**:
- orientato alla connessione: è richiesto un setup fra processi client e server, che permette la “““creazione di un”
- trasporto affidabile
- controllo di flusso
- controllo della congestione
- non offre:
**UDP**
- senza connessione:
- trasporto inaffidabile:
- non offre:

>[!info]- perchè usare UDP ? 
![[Pasted image 20250321115511.png]]
è più veloce, ed è usato quando non interessa che arrivi sempre tutto e in ordine perfetto. 
## UDP