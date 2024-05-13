## output su console
- usando la classe System.out (oggetto dello standard input)
- `out` è un campo statico, pubblico, final di System di tipo java.io.PrintStream (che estende java.io.OutputStream)
java.io.PrintStream è una classe con tanti metodi, di cui `print(Object obj)`, che prende un oggetto e chiama il metodo toString della classe (è importante implementare toString nelle classi create !!)

## input da tastiera
- usando la classe java.util.Scanner costruita con System.in
- `System.in` è un campo statico, publico, final di System, di tipo java.io.InputStream
### java.util.Scanner
si può utilizzare lo scanner anche passando un file o altre fonti invece della console, e per alcuni di questi canali di flussi dati sarà molto importante chiuderli con il metodo close() per evitare comportamenti indesiderati
![[Pasted image 20240513213853.png|500]]