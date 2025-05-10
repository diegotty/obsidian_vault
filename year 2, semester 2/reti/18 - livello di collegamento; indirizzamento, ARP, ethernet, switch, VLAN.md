---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-10T18:57
completed: false
---
# indirizzi MAC
anche chiamato indirizzo **fisico/ethernet/LAN**, è un indirizzo di 6 byte, che rappresenta **univocamente** ciascun **adattore** (scheda di rete) di una LAN
- quando un datagramma passa dal livello di rete al livello di collegamento, viene incapsulato in un frame con una intestazione che contiene gli **indirizzi di collegamento** (MAC) della sorgente e della destinazione del frame (non del datagramma !!)
>[!tip] differenza con indirizzi IP
gli indirizzi IP sono usati a livello di rete, e individuano con esattezza i punti di internet dove sono connessi gli host sorgente e destinazione

>[!info] indirizzi MAC
![[Pasted image 20250510184554.png]]

>[!info] come funziona il biz
![[Pasted image 20250510185415.png]]
notiamo che:
>- ogni stazione (adattatore/scheda di rete) ha un’indirizzo MAC
>- ogni router(/switch ?) ha un indirizzo MAC per ogni interfaccia (porta)
>- l’host inoltra il messaggio all’indirizzo MAC del prossimo hop, non all’indirizzo MAC del destinatario !
>	- ciò accade per ogni hop

