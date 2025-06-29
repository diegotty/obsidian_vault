---
related to: 
created: 2025-03-02T17:41
updated: 2025-06-29T21:22
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
l’elemento neutro di $M_{m,n}$ e $(0)$

## elemento neutro di $\frac{G}{H}$
## equazioni congruenziali
data $aX \equiv b \text{ mod }n$
- controllare se ha soluzione $\text{MCD}(a,n) |b$
- se $a$ non è invertibile mod $n$, $\text{MCD}(a, n) = p > 1$ e possiamo dividere l’intera equazione per $p$
	 - una volta divisa l’equazione per $p$, $a$ sarà invertibile mod $n$
-  se $a$ è invertibile mod $n$ ma trovare l’inverso non è banale, si usa bezout ($ak + nk' = 1$), e $k$ sarà l’inverso di $a$


## cose nuove imparate
- teorema di lagrange su ordine degli elementi di un sottogruppo
- classi laterali in notazione additiva (def facile e difficile)
- trasformazione di resto modulo k da negativo a postivo
- pensare a definizione di classi laterali facile e non difficile, per trovare le classi laterali (+ saper dimostare quella facile)