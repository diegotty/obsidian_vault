---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-09T18:56
completed: false
---
>[!example] esempio di funzionamento del livello rete
![[Pasted image 20250409185452.png]]
>- il livello di rete di H1 prende i segmenti dal livello di trasporto, li incapsula in un datagramma, e li trasmette al router più vicino
>- il livello di rete di H2 riceve i datagrammi da R7, estrae i segmenti e li consegna al livello di trasporto
>- il livello di rete dei nodi intermedi inoltra verso il prossimo router
## funzioni chiave del livello di rete
**instradamento** (routing): determina il percorso seguito dai pachetti dall’origine (costruisce le rotte)
**inoltro** (forwarding): decide su quale porta di uscita deve essere immesso il pacchetto in base a quanto stabilito dal routing (utilizza il percorso definito dal routing)


la porta di uscita viene scelta in base all’IP di destinazione
ogni router esegue il protocollo di routing ! e ha la sua tabella di router
## switch e router
**packet switch**: dispositivo che si occupa del trasferimento dall’interfaccia di ingresso a quella di uscita
1. 

se l’inoltro avviene in base all’indirizzo ip (livello 3), abbiamo un router
se l’introl avviane a info di livello 2 (indirizzo MAC) abbiamo un link layer switch
### link-layer switch
### router


approccio a circuito virtuale


il VC identifica il circuito tra mittente e destinazione


inoltrare ad una interfaccia significa inoltrare ad un router (ogni interfaccia porta ad un router)


tabella di routing PUÒ essere implementata su tutte le porte

### commutazione tramite bus


nelle porte di uscita si possono accodare i pacchetti perhce può capitare che più porte di input mandino pacchetti alla stessa porta di output

DHCP implementa funzioni di livello di rete, ma viene implementato a livello applicazione


identificatore, flag e offset usati per la frammentazione (che è una cosa che si può fare ceh è ok)


protocollo: ICMP< IGMP, OSPF usano tutti il protocollo IP anche se sono a livello di rete

