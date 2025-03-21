---
related to: "[[03 - introduzione allo stack protocollare TCP-IP]]"
created: 2025-03-02T17:41
updated: 2025-03-21T11:26
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
un **socket** è l’unione di indirizzo IP e porta
### incapsulamento/decapsulamento
>[!info] illustrazione !
![[Pasted image 20250321112311.png]]
>i pacchetti a livello di trasporto sono chiamati **segmenti** se viene usato il protocollo TCP, o **datagrammi utente** se viene usato il protocollo UDP
### multiplexing/demultiplexing
**multiplexing** dell’host mittente: raccogliere i dati da varie socket, incapsularli con l’intestazione