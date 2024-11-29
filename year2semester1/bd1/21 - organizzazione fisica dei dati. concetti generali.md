---
created: 2024-11-29
related to: 
updated: 2024-11-29T16:48
---
>[!info] rapprentazione memoria a disco rigido
![[Pasted image 20241129164243.png]]
[[dischi#HDD|come in SO!]], abbiamo testina che ruota, tracce e settori. il bottleneck per noi è il trasferimento dei dati (soprattuto in scrittura), che è molto più lento del tempo di elaborazione della CPU

 per il nostro interesse, faremo delle assuzioni su alcune cose.
 al momento della formattazione del disco, ogni traccia è suddivisa in **blocchi** di dimensione fissa (compresa tra $2^9$ e $2^{12}$ byte)
 >[!important] noi useremo gli accessi in memoria come unità di misura !
 >per accesso si intende il trasferimento di un blocco da memoria secondaria a memoria principale (lettura di un blocco) o da memoria principale a memoria secondaria (scrittura di un blocco)


