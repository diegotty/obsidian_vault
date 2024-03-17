
si possono definire staticamente nella sezione .data del programma assembly !
l’etichetta indicherà l’indirizzo del primo elemento del vettore
[!tuff] per indirizzare l’elemento i-esimo, bisogna aggiungere l’offset
i * dimensione_elemento


## strings
```armasm
label2: .asciiz "sopra la panca"
```
vettore di caratteri (byte) seguiti da /0