---
created: 2024-12-18
related to: 
updated: 2024-12-18T13:29
---
l’area di memoria di un processo caricare in memoria (principale) è diviso nelle sezioni seguenti:
>[!figure] area di memoria di un processo
![[Pasted image 20241218131014.png]]

lo stack è costituito da stack frames, e ciascun frame contiene:
- parametri passati alla funzione
- variabili locali
- indirizzo di ritorno
- instruction pointer
esistono poi lo stack pointer, che punta all’inizio dello stack, e il frame pointer, che punta alla base del frame corrente

in particolare, la chiamata di funzione prosegue in questo modo:
- se ci sono parametri passati alla funzione, sono aggiunti allo stack
- l’indirizzo di ritorno (return address) è aggiunto allo stack
- il puntatore allo stack frame viene salvato sullo stack
- viene allocato spazio ulteriore sullo stack per le variabili locali della funzione chiamata
>[!example] esempio di stack
vediamo uno stack di 2 funzioni: G (funzione corrente) viene chiamata da F
![[Pasted image 20241218132032.png]]

```c
void foo(char *s) {
	char buf[10];
	strcpy(buf, s);
	printf("buf is %s\n", s);
}

foo("stringatroppolungaperbuf");
```
in questo caso, mandiamo come parametro una stringa troppo lunga, che non entra in `buf[10]`. ma il computer non sa la dimensione di `buf`, in quanto vede solo indirizzi di memoria, quindi continua a copiare la stringa negli indirizzi successivi a quelli allocati per `buf`, andando a sovrascrivere qualsiasi cosa trovi, fino a che non completa l’operazione richiesta
>[!example] esempio di evoluzione stack
>ottieniamo quindi questa situazione (tra parentesi troviamo i valori delle aree di memoria dopo `strcpy`)
![[Pasted image 20241218132436.png]]
>
>in realtà, un indirizzo conterrà più lettere, in base alla dimensione delle parole di memoria. ottieniamo quindi una situazione di questo tipo:
![[Pasted image 20241218132617.png]]

## stack smashing
di solito, fare overflow di un buffer in questo modo porta alla terminazione del programma (segmentation fault)
tuttavia, se i dati che sono usati nell’overflow del buffer sono preparati in modo accurato, è possibile eseguire del codice arbitrario:
```c
void foo(char *s) {
	char buf[10];
	strcpy(buf, s);
	printf("buf is %s\n", s);
}

foo("stringatroppolun\xda\x51\x55\x55\x55\x55\x00\x00");
```
