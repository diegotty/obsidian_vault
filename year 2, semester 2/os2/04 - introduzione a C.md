---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-26T22:43
completed: false
---
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
## intro a C
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

>[!info] compilare ed eseguire
>```sh
>gcc prog-name.c
>```
flags:
>- `-Wall`: vengono stampati tutti i messasggi di warning (se ci sono)
>- `-lm`: va specificato se si includono le librerie matematiche `<math.h` (ad esempio per usare funzioni come `sin, cos, log, ln, ...`)
>
>il risultato è un file eseguibile `a.out`
>- `-o nomefile.o` per specificare il nome del file eseguibile