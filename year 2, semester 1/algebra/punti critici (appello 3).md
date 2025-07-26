---
related to: 
created: 2025-03-02T17:41
updated: 2025-07-07T09:41
completed: false
---
## matrici
- prendere i vettori come righe o come colonne
>[!warning] vettori riga = vettori colonna, ma:
>- se prendo dei vettori come vettori colonna, se voglio trovare una base **DEVO** usare gauss sulle colonne (che è uguale a prenderli come righe e usare gauss sulle righe … ), in quanto gauss sulle righe **non conserverebe il sottosp. vett. originale** generato dalle colonne
>	- ma il rango resta lo stesso ! 
>	- se prendo dei vettori come colonne e uso gauss sulle righe, devo prendere, **dalla matrice originale**, le colonne corrispondenti alle colonne dei pivot

## equazioni congruenziali
nella tabella: $y_{i}$ è riducibile mod $r_{i}$, e va poi moltiplicato con $R_{i}$

## applicazioni lineari
>[!info]
![[Pasted image 20250628133918.png]]
l’applicazione è definita in questo modo: prende la matrice $\in M_{4, 1}$
>$$\begin{pmatrix}
a \\
b \\
c \\
d
>\end{pmatrix}$$
ed il risultato è il prodootto di essa con la matrice $\in M_{3,4}$
>$$\begin{pmatrix}
1 & 0 & 1 & 2 \\
0 & 1 & 1 & 0 \\
1 & -1 & 0 &2
>\end{pmatrix}$$
il risultato $\in M_{3,1}$ ($M_{3,4}\times M_{4,1} = M_{3,1}$)
l’elemento neutro di $M_{m,n}$ è $(0)$

## elemento neutro di $\frac{G}{H}$
## equazioni congruenziali
data $aX \equiv b \text{ mod }n$
- controllare se ha soluzione $\text{MCD}(a,n) |b$
- se $a$ non è invertibile mod $n$, $\text{MCD}(a, n) = p > 1$ e possiamo dividere l’intera equazione per $p$
	 - una volta divisa l’equazione per $p$, $a$ sarà invertibile mod $n$
-  se $a$ è invertibile mod $n$ ma trovare l’inverso non è banale, si usa bezout ($ak + nk' = 1$), e $k$ sarà l’inverso di $a$


## cose nuove imparate
### 29/06/2025
- teorema di lagrange su ordine degli elementi di un sottogruppo (dividono l’ordine del sottogruppo)
- classi laterali in notazione additiva (def facile e difficile)
- trasformazione di resto modulo k da negativo a postivo
- pensare a definizione di classi laterali facile e non quella difficile, per trovare le classi laterali (+ saper dimostare quella facile)
### 30/06/2025
- abbiamo studiato i polinomi costruiti da campi, in quanto tali anelli hanno una struttura algebrica ben definita (banalmente: divisione è sempre possibile)
	- nei campi è sempre possibile dividere un numero per un qualsiasi $a \neq 0$ (in quanto dividere per $a$ significa moltiplicare per $a^{-1}$, che nei campi esiste sempre, in altri anelli no !!)
- dimostrazione che $Z$ non è un campo
- $H \leq G \implies H=Ker(\pi_{H})$ (dimostrazione)
### 02/07/2025
- capita di poter rispondere alle domande di es. su spazi vett. semplicemente ragionando, senza fare calcoli !
- coniugazione di permutazioni: due permutazioni $\sigma, \tau$ sono coniugate $\iff$ $\exists \pi \,\,t.c. \sigma= \pi \tau \pi^-1$
	- aka due permutazioni sono coniugate se hanno la stessa struttura ciclica (non nello stesso ordine perchè si da per scontato che i cicli siano disgiunti)
- tutti i sottogruppi di un gruppo ciclico sono ciclici
- modo per trovare tutti i sottogruppi di $Z/nZ$ (pensando all’ordine degli elementi generatori dei vari sottogrupppi)
- esiste una nozione di invertibiltà rispetto alla somma negli anelli unitari, ed è sempre verificata ($\forall a, \exists b  \,\, t.c \,\,a-b = 0$)
	- noi generalmente parliamo di invertibilità moltiplicativa (esiste un elemento che moltiplicato da 1)
- ordine di un elemento invertibile (???????)
### 03/07/2025
- vera struttura di spazio vettoriale e sottospazio vettoriale ! non fare confusione tra K e V
	- di Im e Ker sottosp. vett. di un’applicazione lineare
- i gruppi ciclici sono abeliani (scontato ma mai detto esplicitamente)
- usare classi di eq per elementi negativi in divisione tra polinomi
### 04/07/2025
- dato un gruppo ciclico, tutti i suoi sottogruppi sono sottogruppi generati da uno dei suoi elementi !!!
- data una matrice, anche se gli spazi vettoriali generati dai vettori presi come righe e colonne sono diversi, i due spazi vettoriali condividono la dimensione
	- spiegazione: $rg(A)=rg(A^t) \implies$ il rango deve essere lo stesso x righe e colonne il rango deve essere lo stesso x righe e colonne
	- torniamo al discorso che applicando gauss sulle colonne, lo spazio vettoriale generato dalle colonne viene alterato
### 06/07/2025
>[!warning] matrici parametriche
>**NON POSSIAMO SEMPRE MOLTIPLICARE PER $x$, nè divdere per $x$ se non assumiamo che $x \neq 0$ (o in generale dividere/ moltipilcare per un’espressione contenente $x$ che si potrebbe annullare)**
>- se ci serve moltiplicare per $x$, dobbiamo gestire 2 casi:
>	- il caso in cui l’espressione non si annulla (andando avanti normalmente)
>	- il caso in cui l’espressione si annulla (non applicando l’operazione ma sostituendo il parametro riducendo fino alla fine )
 >
> **logica dietro matrici con parametri:** devo provare ad annullare i pivot (sostituendo $x$ con un valore adeguato), ma devo provare a farlo solamente quando $x$ è necessariamente un pivot !! cioè quando il resto della matrice è a gradini e il prossimo pivot deve essere quello che ha primo valore = $x$
 