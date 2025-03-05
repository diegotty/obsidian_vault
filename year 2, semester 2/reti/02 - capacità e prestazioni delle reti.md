---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-05T22:35
completed: false
---
# Internet
Internet è una rete di reti, composta da reti di accesso e dal **backbone Internet**
## rete di accesso
(o **access network**)
collegamenti fisici, che connetto un sistema al primo **edge router**
- quindi collegano il sistema di origine al primo router sul percorso per il sistema di destinazione
le modalità sono state viste in [[01 - introduzione alle reti#accesso ad internet| accesso ad internet]]
## backbone Internet
è la rete composta esclusivamente da router e collegamenti tra router. può essere a commutazione di circuito, o a commutazione di pacchetto ! [[01 - introduzione alle reti#switching|(switching !)]]
## struttura di Internet
Internet ha una struttura fondamentalmente gerarchica:
- **ISP di livello 1**: al centro, offrono copertura nazionale/internazionale, e comunicano tra di loro come pari
>[!info]- dorsali sottomarine
>sono stati posati cavati sottomarini (?), per connettere diversi continenti a velocità elevate ! 
>esempi: TAT-14, MAREA, SEA-ME-WE 6 (foto sottostante)
![[Pasted image 20250305223100.png]]

- **ISP di livello 2**: ISP nazionali o distrettuali, e si possono connettere solo **ad alcuni** ISP di livello 1, e possibilmente ad altri ISP di livello 2
>[!info]- situazione monetaria
>un ISP di livello 2 paga l’ISP di livello 1 che gli fornisce la connettività per il resto della rete ! un ISP di livello 2 è quindi un cliente di un ISP di livello 1

**ISP di livello 3** e **ISP locali**: anche chiamate **last hop networks**, sono le più vicine ai sistemi terminali
- sono clienti degli ISP di livello superiore

>[!example] in pratica !
possibile routing di un pacchetto !
![[Pasted image 20250305223454.png]]
# capacità e prestazioni delle reti