---
created: 2024-12-09
related to: 
updated: 2024-12-09T10:59
---
proviamo ora a gestire la mutua esclusione senza aiuto dal parte dell’hardware o dal SO. gestiremo quindi tutto nel codice (senza la sicurezza di avere operazioni atomiche).
>[!important] le soluzioni che vedremo valgono per 2 processi
>fare il passaggio a $n$ processi è possibile ma non semplice

>[!info] primo tentativo
>![[Pasted image 20241209102943.png]]
>- lame, se un processo è da solo e vuole accedere alla sezione critica non lo può fare. L
>- busy-waiting !! L

>[!info] secondo tentativo
>![[Pasted image 20241209103106.png]]
uso un array (di dimensione 2, sempre per 2 processi), per indicare se uno dei 2 processi è in sezione critica. P1 legge il valore del flag di P2 e scrive il suo valore, e P2 legge il valore del flag di P1 e scrive il suo valore.
>- all’inizio entrambi i valori di flag sono inizializzati a 0, quindi se il dispatcher gestisce con interleaving perfetto, entrambi i processi entrano in sezione critica

>[!info] terzo tentativo
![[Pasted image 20241209103438.png]]
uso il flag per comunicare l’intenzione di voler accedere in sezione critica
>- con interleaving perfetto, deadlock comunque. 

>[!info] quarto tentativo
>![[Pasted image 20241209103538.png]]
>l’idea è che con interleaving perfetto, entro nel loop e usando un buon delay, riesco a settare il mio flag a true prima che l’altro processo possa fare lo stesso.
>potrebbe funzionare con buona probabilità, ma non generale abbastanza.
>livelock:
>![[Pasted image 20241209105858.png|250]]


