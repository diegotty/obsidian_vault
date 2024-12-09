---
created: 2024-12-09
related to: 
updated: 2024-12-09T08:54
---
# trastevere (roma)
>[!info] roma mia quanto sei bella
>![[Pasted image 20241208182157.png]]
>- i blocchetti sono le macchine, il tubo è la strada di trastervere
>- la strada si restringe: senso unico alternato, massimo 4 auto per volta
>- vince chi arriva prima, non ci può essere parità
>- una volta impegnata da un lato, tutte le macchine dell’altro lato devono aspettare che non ci siano più macchine nè sulla strettoia, nè in coda
>- amico mio, vedi ad andare in macchina a trastevere, ma che brutta idea che ti è passata per la mente
>- si ma gli autobus puzzano, io ho l’arbre magique del pupone

```c
semaphore z = 1;
semaphore strettoia = 4;
semaphore sx = 1;
semaphore dx = 1;
//4 semafori (sai com'è, a roma, alla fine ne fuziona 1)
int nsx = 0; // curr numero di macchine in coda a sinistra
int ndx = 0; // curr numero di macchine in coda a destra

macchina_dal_lato_sinistro () {
	wait(sx);
	nsx++;
	if(nsx == 1)
		wait(dx);
	signal(sx); 
	signal(z);
	
	wait(strettoia);
	passa_strettoia();
	signal(strettoia);
	wait(sx);
	nsx--;
	if(nsx == 0)
		signal(dx);
	signal(sx);
}
```

>[!warning] se non c’è mutua esclusione c’è un incidente frontale
## breakdown
`z`: semaforo che ci permette di evitare il deadlock
`strettoia`: semaforo che garantisce il passaggio delle macchine attraverso la strettoia (viene inizializzato a 4 perchè nella strettoia possono trovarsi 4 macchine)
`sx,dx`: semafori usato per garantire la mutua esclusione nell’entrata nella strettoia
`nsx,ndx`: rispettivamente: current numero di macchine in coda a sinistra, current numero di macchine in coda a destra

- il seguente snippet di codice, garantisce che le (al massimo) 4 macchine che sono nella strettoia riescano a passare, mentre la quinta si fermi (`count < 0`)
```c
	wait(strettoia);
	passa_strettoia();
	signal(strettoia);
```

- notiamo come la funzione `macchina_dal_lato_sinistro` incrementi e decrementi solo `nsx` (come è giusto che sia, in quanto una macchina dal lato sinistro non ha informazioni sul numero di macchine in coda dal lato destro)
- il seguente snippet di codice in `macchina_dal_lato_sinistro` serve perchè se viene eseguito dalla prima macchina in coda nel lato sinistro (quindi `nsx == 1`), deve bloccare le macchine dal lato destro dall’entrare nella strettoia. allo stesso modo, se viene eseguito dall’ultima macchina in coda (dopo il decremento `nsx == 0`), essa deve sbloccare le macchine dal lato destro, per farle entrare nella strettoia
```c
if(nsx == 1)
	wait(dx);
/////
if(nsx == 0)
	signal(dx);
```
funziona perchè se la prima macchina a passare è da sinistra, il wait sopra non fermerà le macchine da destra, ma il `wait(dx)` chiamato dalla prima macchina da destra si !

ricordiamo che il semplice incremento
```c
++nsx;
```
comporta una piccola race condition (in quanto, in verità, è più istruzioni assembly)(esempio sopra) /TODO

```c
wait(sx);
signal(sx);
```
sono usati per garantire una mutua esclusione sulla variabile globale `nsx`
la macchina dal lato destro fa esattamente la stessa cosa, ma scambiando `sx`

senza la presenza di `wait(z);`, è possibile avere un deadlock: S1 e D1 eseguono entrambe, rispettivamente `wait(sx)` e `wait(dx)`, e quando S1 arriva a `wait(dx)`, si blocca perchè `dx == -1`. lo stesso accade per D1, che arriva a `wait(sx)` e va in `BLOCKED`

il semaforo `z` impedisce che lo scheduling sopra avvenga, poichè il bloco tra `wait(z)` e `signal(z)` sarà eseguibile solo da un processo alla volta

# il negozio del barbiere