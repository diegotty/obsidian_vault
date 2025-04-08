---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-08T19:55
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
# variabili
sappiamo cos’è una variabile, poco da dire ….
`option_modifier data_type name_list;`
- **option_modifier**: `signed`, `unsigned`, `short`, `long`, `const`
- **data type**: specifica il tipo di valore, permette al compilatore di sapere quali sono le operazioni consentite sul tipo di dato e come deve essere rappresentato in memoria
- **name list**: lista di nomi di variabili
## tipi di variabili
>[!info] data types for numbers
![[Pasted image 20250329201811.png]]
>>[!info] characters + modifiers
>![[Pasted image 20250329201845.png]]

>[!info] booleani
>- `_Bool`: può memorizzare solo 0 e 1 (qualunque valore diverso da 0 viene memorizzato come 1)
>- `bool`: memorizza `true/false` (richiede l’uso di `<stdbool.h>)
>	- 0 significa `false`, 1/diverso da 0 significa `true`

>[!info] stringhe
>- ultimo elemento dell’arrawy contiene carattere di fine stringa: `\0`(detto `NULL`)
## operatori
fun facts:
- `/`: il risultato è dello stesso tipo di dato dell’operando più grande (in termini di tipo di dato)
### dichiarazione di variabili
le variabili locali possono essere dichiarate tutte all’inizio di una funzione o possono essere dichiarate nel punto più vicino al loro primo uso.
esistono dei vantaggi per entrambi:
**dichiarazione all’inizio della funzione**:
- vantaggi:
	- **historical context**: era una convenzione all’inizio !
	- **memory allocation**: dichiarare variabili all’inizio della funzione permette al compilatore di allocare la memoria per tutte le variabili una sola volta, il che può migliorare performance. il layout della memoria è anche più semplice e può essere ottimizzato più facilmente
	- **scope visibiity**: lo scope delle variabili è più chiaro e prevedibile 
	- **error prevention**: la dichiarazione all’inizio può evitare errori riguardo a uso prima della dichiarazione
	- **code clarity**: il codie è più leggibile
- svantaggi:
	- promuove il riuso di variabili per scopi differenti, che non è una buona abitudine
**dichiarazione il più vicino possibile al punto di primo uso**:
- vantaggi:
	- riduce la vita delle variabili al minimo necessario
	- non promuove il riuso 
- svantaggi: 
	- poco ordinato

| placeholder | tipo                                                                              |
| ----------- | --------------------------------------------------------------------------------- |
| `%d/%i`     | integer                                                                           |
| `%l`        | long                                                                              |
| `%o`        | integer in ottale                                                                 |
| `%x`        | integer in esadecimale                                                            |
| `%f/%e/%g`  | float, rispettivament: formato standard, notazione scientifica, scelta automatica |
| `%lf`       | double                                                                            |
# input e output
l’ambiente run-time di C, quando un programma viene eseguito, apre 2 file: `stdin` e `stdout`
- tutte le funzioni essenziali per l’I/O sono nel file `stdio.h`
## output
```c
printf("format string", value-list);
```
- `value-list` può contenere sequenze di caratteri, variabili, costanti, espressioni logico-matematice
- `printf` riceve valori, ma C permette di manipolare anche indirizzi di memoria e passarli come input a funzioni (anche se per stampare il contenuto di una locazione di memoria, si usa `scanf`)
>[!figure] format string values
![[Pasted image 20250326231401.png]]
### output di variabili
`format_string` (guarda sopra) deve contenere dei **placeholder**. un placeholder inizia con % e dice che al suo posto ci andrà il valore di una variabile e che tipo di dato deve essere scritto
>[!example] esempio
 `printf("%d, %l", integerVar, longval);`

## input
```c
scanf(format-string, address-list);
```
- `format-string`: contiene placeholder che dicono a `scanf` in che tipo di dato la stringa input viene convertita
- `address_list`: contiene gli indirizzi di memoria in cui devono essere memorizzati i valori ricevuti in input
 - restituisce come risultato il numero di valori di input letti
 >[!example] esempio
 `scanf("%d", &peso);` 
 > - `peso` è una variabile intera, `&` estrae il suo indirizzo di memoria e lo passa a `scanf`
## stringhe
array di carattere con `NULL` (`\0`)come ultimo elemento
>[!info] carattere `\0`
![[Pasted image 20250408172419.png]]
il carattere `\0` viene aggiunto automaticamente se inizializzato come nel primo modo, ma non nel secondo !
>
>inoltre se scriviamo `char r[10] = "L9 4apr"`, `r[7] = \0`, mentre `r[8]` è indeterminato
>- in questo caso conviene scrivere `char r[] = "L9 4apr"`, in quanto verrà allocata una stringa di dimensione 8

`<string.h>` contiene funzioni e macro per la gestione delle stringhe
### funzioni 
- `char *strcpy(char *dest, const char *src)`: copia stringa `src` nella stringa `dest`
	- `char *strncpy(char *dest, const char *src, size_t n)`: uguale, ma al più `n` byte sono copiati
- `int strcmp(const char *s1, const char *s2)`: confronta il contenuto di `s1` con quello di `s2`. restituisce 
	- `0 if s1 == s2`
	- `<0 if s1 < s2`
	- `>0 se s1 > s2`
- `char *strcat(char *dest, const char *src)`: concatena il contenuto della stringa `src` con quello di `dest` e mette la nuova stringa in `dest`