---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-04T11:08
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
```python
def Fib(n):
	if n <= 1: return 1
	a = Fib(n-1)
	b = Fib(n-2)
	return a + b
```
l’equazione di ricorrenza per il tempo di calcolo dell’algoritmo è: 
$$
T(n) = T(n-1) +  T(n-2) + O(1)
$$
dove ovviamente
$$
T(n) \geq 2T(n-2) + O(1)
$$
che, risolta, produce $T(n)\geq \Theta(2^{\frac{n}{2}})$