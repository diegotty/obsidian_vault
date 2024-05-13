## output su console
- usando la classe System.out (oggetto dello standard input)
- `out` è un campo statico, pubblico, final di System di tipo java.io.PrintStream (che estende java.io.OutputStream)
java.io.PrintStream è una classe con tanti metodi, di cui `print(Object obj)`, che prende un oggetto e chiama il metodo toString della classe (è importante implementare toString nelle classi create !!)

## input da tastiera
- usando la classe java.util.Scanner
- `in` è un campo statico, publico, final di System, di tipo java.io.InputStream