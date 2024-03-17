
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
i byte della word sono memorizzati dal **least** signficant byte al **most** signfiicant b
![[Pasted image 20240317175138.png]]