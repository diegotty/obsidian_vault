## principio della località
- principio di località temporale: un programma tende ad accedere allo stesso elemento in momenti temporali vicini tra loro
- principio di località spaziale: un programma tende ad accedere successivamente a elementi di memoria vicini tra loro
per sfruttare al massimo questi due principi, interponiamo una memoria piccola, la **cache** tra la CPU e la memoria, in cui conserveremo:
- i dati più usati (principio di località temporale)
- i dati vicini a quelli più usati (principio di località spaziale)


## schema e terminologia
- la memoria RAM è divisa in blocchi, e ogni blocco a X word

- la cache è divisa in blocchi di dimensioni uguali
- la cache contiene un numero N di linee ( o set)

## hit e miss
quando la CPU richiede un indirizzo di memoria, appartenente ad un blocco (????), le possibilità sono 2:
- **MISS**: il blocco è stato caricato nella cache, dunque il dato all’indirizzo richiesto viene restituito immediatamente
- **HIT**: il blocco non è presente nella cache, il dato viene richiesto alla RAM, caricando all’interno della cache **l’intero blocco** a cui esso appartiene

## cache direct-mapped
poichè una cache deve contenere solo i dati più richiesti, utilizzando dimensioni limitate, è necessario che più blocchi di memoria vengano salvati nello stesso spazio, sovrascrivendosi a vicenda
la cache direct-mapped viene strutturata in questo modo:
![[Pasted image 20240603095007.png]]
- N linee, corrispondenti agli spazi occupabili dai blocchi. ogni linea è composta da:
	- un bit di validità: indicante se i dati contenuti nella linea siano validi o meno.
	- un campo tag: **in grado di distinguere quale blocco della memoria sia caricato nella linea**. tale campo risulta fondamentale poichè più blocchi di memoria vengono mappati sulla stessa linea. grazie al tag si previene la lettura del blocco sbagliato
	- il blocco stesso, memorizzato all’interno della linea