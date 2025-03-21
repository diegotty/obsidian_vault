---
related to: "[[03 - introduzione allo stack protocollare TCP-IP]]"
created: 2025-03-02T17:41
updated: 2025-03-21T11:09
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