---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-04T11:20
completed: false
---
sappiamo che gli algoritmi basati sulla tecnica divide_et_impera seguono 3 passi:
1. dividi il problema in sottoproblemi di taglia inferiore
2. risolvi (ricorsivamente) i sottoproblemi di taglia inferiore
3. combina le soluzioni dei sottoproblemi in una soluzione del problema originale
>[!tip]
negli esempi visti finora, i sottoproblemi che si ottenevano dall’applicazione del passo 1 erano tutti **diversi**, pertanto ciascuno di essi veniva individualmente risolto dalla relativa chiamata ricorsiva del passo 2. in molte situazioni però, i sottoproblemi ottenuti al passo 1 possono risultare **uguali**. in tal caso, l’algoritmo basasto sulla tecnica *divide_et_impera* **risolve più volte lo stesso problema, svolgendo lavoro inutile**

>[!example] esempio : er fibonacci
scriviamo un algoritmo che, dato un intero $n$, calcola $f_{n}$ (con $f_{i} = f_{i-1}+f_{i_{2}}$, e $f_{0}=f_{1} = 1$)
>
il primo algoritmo che viene in mente per risolvere il problema è basato sul *divide_et_impera* e sfrutta la definizione di numero di fibonacci:
>```python
>def Fib(n):
>	if n <= 1: return 1
>	a = Fib(n-1)
>	b = Fib(n-2)
>	return a + b
>```
>l’equazione di ricorrenza per il tempo di calcolo dell’algoritmo è: 
>$$
>T(n) = T(n-1) +  T(n-2) + O(1)
>$$
>dove ovviamente
>$$
>T(n) \geq 2T(n-2) + O(1)
>$$
>che, risolta, produce $T(n)\geq \Theta(2^{\frac{n}{2}})$

>[!info] il problema
>il motivo di tale inefficienta sta nel fatto che il programma `Fib(n)` viene chiamato più volte sullo stesso input
>![[Pasted image 20250504111025.png]]
i sottoproblemi in cui viene scomposto il problema **non sono disgiunti**: questo fenomeno prende il nome di **overlapping di sottoproblemi**.
>- ma l’algoritmo di somporta come se fossero nuovi, e li calcola nuovamente
>
>per risolvere l’overlapping, basta memorizzare in una lista i valori $fib(i)$ quando li si calcola per la prima volta, cosicchè nelle chiamate ricorsive a $fib(i)$ non ci sarà più bisogno di ricalcolarli  ma potranno essere ricavati dalla lista. stiamo applicando quindi la tecnica della **memoizzazione**
>```python
>def Fib(n):
>	F = [-1]*(n+1)
>	return memFib(n, F)
>
>def memFib(n, F):
>	if n <= 1: return 1
>	if F[n] == -1:
>		a = memFib(n-1, F)
>		b = memFib(n-2, F)
>		F[n] = a + b
>	return F[n]
>```
>in questo modo l’algoritmo effettuerà **esattamente $n$ chiamate ricorsive**, e tenendo conto cche ogni chiamata ricorsiva costa $O(1)$, il tempo di calcolo di `Fib(n)` è $\Theta(n)$: un miglioramento esponenziale rispetto alla versione da cui eravamo partiti !!
>
PS: 
>- è possibile eliminare la ricorsione per ottenere la versione iterativa, in cui la complessità asintotica rimane $O(n)$ ma c’è un risparmio di tempo e spazio che era necessario per gestire la ricorsione
>- è possibile ridurre la complessità di spazio, inzialmente di $\Theta(n)$, tenendo conto che dell’intera tabella basta conservare in memoria, volta per volta, solo gli ultimi due valori calcolati
