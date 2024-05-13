
# generici
i tipi generici sono un modello di programmazione che permette di definire, con una sola dichiarazione, un intero insieme di metodi o classi
è un meccanismo molto potente, da usare con consapevolezza !

## esempio di classe generica 
```java
public class Valore<T>{
	public final T val;
	
	public Valore(T val) { this.val = val; }

	public T get() { return val; }

	public String toString() { return ""+val;}

	public String getType() { return val.getClass().getName();}
	
} 
public static void main(String[] args){
	Valore<Integer> i = new Valore<Integer>(42); //getType ritorna Integer
	Valore<Valore<Integer>> v = new Valore<>(i); //getType ritorna Valore
// si può omettere il tipo nella parte destra dell'inizializzazione !

}
```
per convenzione i tipi generici sono chiamati con le lettere T,S , etc (E nel caso in cui siano elementi di una collection
## caratteristiche 
- non è possibile utilizzare i primitivi come argument dei tipi generici
```java
Valore<int> v; //da errore
//versioni future di java potrebbero permetterlo
```

- i tipi generici differiscono sulla base dei loro tipi(Valore\<Integer> non è compatibile con Valore\<String>)