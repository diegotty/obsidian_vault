---
created: 2024-11-29
related to: 
updated: 2024-11-29T17:04
---
>[!info] rapprentazione memoria a disco rigido
![[Pasted image 20241129164243.png]]
[[dischi#HDD|come in SO!]], abbiamo testina che ruota, tracce e settori. il bottleneck per noi è il trasferimento dei dati (soprattuto in scrittura), che è molto più lento del tempo di elaborazione della CPU

 per il nostro interesse, faremo delle assuzioni su alcune cose.
 al momento della formattazione del disco, ogni traccia è suddivisa in **blocchi** di dimensione fissa (compresa tra $2^9$ e $2^{12}$ byte)
 >[!important] noi useremo gli accessi in memoria come unità di misura !
 >per accesso si intende il trasferimento di un blocco da memoria secondaria a memoria principale (lettura di un blocco) o da memoria principale a memoria secondaria (scrittura di un blocco)
 >in particolare, il tempo necessario per un accesso è dato dalla somma di: 
 >- tempo di posizionamento (della testina sulla traccia in cui si trova il blocco)
 >- tempo di rotazione (rotazione necessaria per posizionare la testina all’inizio del blocco)
 >- tempo di trasferimento (dei dati contenuti nel blocco)


## file
nel nostro caso, un file è un insieme omogeneo di record, construiti in base alla nostra tabella. il file è quindi la struttura di memorizzazione della tabella
inoltre, in ogni file ci sono record appartenenti ad un’unica relazione(ad una sola tabella)
## record
i record corrispondono alla tuple della nostra tabella, e oltre ai campi che corrispondono agli attributi, ci possono essere altri campi che contengono **informazioni sul record stesso** o **puntatori ad altri record/blocchi**
in particolare, all’inizio di un record alcuni byte possono essere utilizzati per:
- specificare il tipo del record (**è necessario quando in uno stesso blocco sono memorizzati record di tipo diverso, cioè record appartenenti a più file**)
- specificare la lungheza del record (se il record ha campi a lunghezza variabile (varchar in sql))
contenere un bit di “cancellazione” o un bit di “usato/non usato” (tipo dirty bit ?)

all’inizio del file potremmo avere un insieme di 