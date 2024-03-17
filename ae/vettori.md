
si possono definire staticamente nella sezione .data del programma assembly !
l’etichetta indicherà l’indirizzo del primo elemento del vettore
[!tuff] per indirizzare l’elemento i-esimo, bisogna aggiungere l’offset
i * dimensione_elemento


## strings
```armasm
label2: .asciiz "sopra la panca"
```
vettore di caratteri (ongi carattere è un byte) seguiti da /0 al termine 

## ordinamento byte di word

il processore MIPS permette l’ordinamento dei byte di una word in 2 modi:
### big-endian
i byte della word sono memorizzati dai **most** signficant byte al **last** significant byte
![[Pasted image 20240317175058.png]]
è l’ordinamento più intuitivo (il vettore contiene 1, 2, 3, 4, 5, 6)
### little-endian
i byte della word sono memorizzati dal **least** signficant byte al **most** signficant byte 
![[Pasted image 20240317175138.png]]
molto meno intuitivo (il vettore contiene 1, 2, 3, 4, 5, 6)
![tuff] mars usa l’ordinamento little-endian !!

more on slides 05-11

## accesso agli elementi per indice
esistono 2 modi per scorrere i vettori nei cicli:
### scansione per indice
tengo traccia dell’indice, e ogni volta che devo accedere agli elementi mi calcolo l’offset corrispondente
pro:
- comodo se si deve usare l’indice per controlli o altro
- l’incremento dell’indice non dipende dalla dimensione degli elementi !
- comodo se il vettore è allocato staticamente
contro:
- ogni volta che devo accedere agli elementi bisogna calcolare l’offset corrispondente
### scansione per puntatore
manipolo direttamente il registro che indica l’indirizzo in memoria
pro:
- si lavora direttamente su indirizzi di memoria
- meno calcoli nel ciclo
contro:
- non si ha a disposizione l’indice dell’elemento
- l’incremento del puntatore dipende dalla dimensione degli elementi(non so che contro sia in verità)
- bisogna calcolare l’indirizzo successivo all’ultimo elemento (per uscire dal ciclo)