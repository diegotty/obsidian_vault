---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-26T23:18
completed: false
---
# intro a C
## ambiente di sviluppo ed esecuzione
noi useremo **gcc**(GNU compiler collection), che include il **compilatore** di C, che svolge le attività di:
- pre-processing
- compilazione
- linking
>[!figure] C development workflow
![[Pasted image 20250326222525.png]]
>i passi dello sviluppo di un programma C

>[!warning] differenze con Java e python
la compilazione di un programma in java non produce un codice eseguibile, ma codice interpretabile dalla JVM. 
la “compilazione” di un programma in C mediante **gcc** invece, produce un “object code” che è un file eseguibile
>- il file eseguibile è quinidi indipendente dal **gcc**, ed eventualmente eseguibile anche su altri sistemi senza necessità di ri-compilarlo (mentre un file .class dovrà sempre essere dato in pasto alla JVM)
>- ciò vale anche per un programma scritto in C++

>[!figure] C ambiente di esecuzione
![[Pasted image 20250326222745.png]]
## primo programma in C
>[!info] primo programma
>```c
>#include <stdio.h>
>
>int main(){
>	printf("scrivi quello che vuoi !!! \n");
>	return 0;
>}
>```
>con `#` si indicano le direttive al preprocessore
>`stdio.h` è un **file header**(`.h`) che contiene costanti, funzioni per input, output e file handling
>- `<>` indicano che il file header è un file standard del C in `/usr/include`
>- `""` indicano che il file header è dell’utente e si trova nella directory corrente o in un path specificato
>- `-I` permette di specificare le directory in cui cercare gli header file

### compilare ed eseguire
per **compilare ed eseguire**: `gcc -Wall prog-name.c -o executable-name.o` (per compilare), `./executable-name.o` (per eseguire)
flags per `gcc`:
- `-Wall`: vengono stampati tutti i messasggi di warning (se ci sono)
- `-lm`: va specificato se si includono le librerie matematiche `<math.h` (ad esempio per usare funzioni come `sin, cos, log, ln, ...`)
per **solo precompilare/preprocessare** un file: `cpp helloworld.c > precompilato.c`
- in questo modo, vegnono eseguite tutte le direttive del compilatore; vengono eliminati i commenti
per **solo compilare** (un file precompilato): `gcc -c precompilato.c -o compilato.o`
- in questo modo, viene controllato che la sintassi sia corretta
- per ogni chiamata a funzione, viene controllato che venga rispettato il corrispettivo header (che quindi deve esistere al momento della chiamata)
- crea effettivamente del codice macchina, ma solo per il contenuto delle funzioni
	- ogni chiamata a funzione ha una destinazione simbolica
per **solo precompilare+compilare**(sostituisce i due step di sopra): `gcc -c file.c -o file.o`
per **solo linkare** il file: `gcc file.o`
- in questo modo, vengono risolte tutte le chiamate a funzioni: durante questo passo, per ogni funzione chiamata non basta più l’header, ma ci deve essere anche l’implementazione, che può essere data dal programmatore o fornita da librerie di sistema
- l’inclusione delle librerie può essere automatica o specificata dall’utente (es: la libreria `libc.a` che contiene `printf()` è incluse automaticamente)
per **linkare più file**: `gcc file1.o file2.o file3.o`
- si possono anche mischiare file `.c` e `.o` !
per fare **tutto**: `gcc file.c`/`gcc file1.c ... filen.c`
## input e output
l’ambiente run-time di C, quando un programma viene eseguito, apre 2 file: `stdin` e `stdout`
- tutte le funzioni essenziali per l’I/O sono nel file `stdio.h`
### output
```c
printf("format string", value-list);
```
- `value-list` può contenere sequenze di caratteri, variabili, costanti, espressioni logico-matematice
- `printf` riceve valori, ma C permette di manipolare anche indirizzi di memoria e passarli come input a funzioni (anche se per stampare il contenuto di una locazione di memoria, si usa `scanf`)
>[!figure] format string values
![[Pasted image 20250326231401.png]]
## variabili
sappiamo cos’è una variabile, poco da dire ….
### dichiarazione di variabili
le variabili locali possono essere dichiarate tutte all’inizio di una funzione o possono essere dichiarate nel punto più vicino al loro primo uso.
esistono dei vantaggi per entrambi: