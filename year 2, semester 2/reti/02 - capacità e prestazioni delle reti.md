---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-05T23:52
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
nel caso di una rete a commutazione di pacchetto, le metriche che ne determinano le prestazioni si misurano in termini di:
- **ampiezza di banda**
- **bit rate**
- **throughput**
- **latenza (ritardo)**
- **perdita di pacchetti**
### ampiezza di banda
si indicano 2 concetti leggermente diversi, ma strettamente legati:
- **bandwidth**: rappresenta la larghezza dell’intervallo di frequenze utilizzato dal sistema trasmissivo (ovvero l’intervallo di frequenze che un mezzo fisico consente di trasmettere senza danneggiare il segnale in maniera irrecuperabile). 
	- maggiore è l’ampiezza di banda, maggiore è la quantità di informazione che può essere veicolata attraverso un mezzo trasmissivo
	- si misura in Hz
- **bit rate**: quantità di bit al secondo (bps) che un link garantisce di trasmettere

il bit rate è proporzionale alla banda, ma dipende anche dalla specifica tecnica di trasmissione, o formato di modulazione digitale
>[!info] per banda di un tipo di rete si intende il bit rate garantito dai suoi link
>il rate di un link Fast Ethernet è 100 mpbs, può inviare al massimo 100 mbps

### throughput
indica quanto velocemente riusciamo **effettivamente** a inviare dati tramite una rete: è quindi il numero di bit al secondo che passano attraverso un punto della rete
>[!warning] bit rate e throughput
>il rate è una misura della potenziale velocità di un link, mentre il throughput è una misura dell’effettiva velocità di un link (irl)

in un percorso da una sorgente a una destinazione, un pacchetto può passare attraverso numerosi link, ognuno con throughput diverso. bisogna calcolare il throughput **end-to-end** (totale)
>[!example] throughput su un percorso con 3 link
![[Pasted image 20250305234547.png]]
notiamo come link3 è il **collo di bottiglia** (collegamento su un percorso punto-punto che vincola il throughput end-to-end)
in questo esempio, il throughput dei dati per il percorso è 100 kpbs

in generale, in un percorso con $n$ link in serie, abbiamo:
$$
\text{throughput = min}\{T_{1},T_{2}, \dots,T_{n}\}
$$
>[!info] thoughput nei link condivisi
>il link tra due router è spesso dedicato a più di un flusso di dati, e raccoglie il flusso da varie sorgenti e/o lo distribuisce a varie destinazioni. il rate del link tra due router è quindi condiviso tra flussi di dati
>>[!example] esempio
![[Pasted image 20250305235205.png]]
crazy nesting grazie aglaia
