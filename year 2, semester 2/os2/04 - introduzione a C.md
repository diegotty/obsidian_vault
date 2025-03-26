---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-26T22:58
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
>`stdio.h` è un **file header**(`.h`) che contiene costanti, funzioni per input, output e file handling
>- `<>` indicano che il file header è un file standard del C in `/usr/include`
>- `""` indicano che il file header è dell’utente e si trova nella directory corrente o in un path specificato

### compilare ed eseguire

per **compilare ed eseguire**: `gcc `
flags:
- `-Wall`: vengono stampati tutti i messasggi di warning (se ci sono)
- `-lm`: va specificato se si includono le librerie matematiche `<math.h` (ad esempio per usare funzioni come `sin, cos, log, ln, ...`)

il risultato è un file eseguibile `a.out`
- `-o nomefile.o` per specificare il nome del file eseguibile

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
## direttie al preprocessore (#)