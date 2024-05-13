
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
## perchè sono utili ?
utilizzare Object fa perdere ogni informazione sul tipo utilizzato, e costringerebbe a continui downcast
## caratteristiche 
- non è possibile utilizzare i primitivi come argument dei tipi generici
```java
Valore<int> v; //da errore
//versioni future di java potrebbero permetterlo
```

- i tipi generici differiscono sulla base dei loro tipi(Valore\<Integer> non è compatibile/convertibile con Valore\<String>)
- quando si estendono classi generiche, si deve inserire il tipo
## vincolo di comparabilità
```java
public interface MinMax<T extends Comparable<T>>{ 
//sto vincolando T a essere un comparabile di T(se stesso) o un sottotipo
	T min();
	T max();	
}

public class MyClass<T extends Comparable<T>> implements MinMax <T>{
// scrivo che T extends Comparable<T> per essere sicuro di poter implementare MinMax !! altrimenti potrei non esserne capace
}
```

>[!tuff] TUTTE LE T SONO LO STESSO TIPO !!

```java
public class ViolenzaSuUnArrayList{
	public static void main(String[] args){
		ArrayList<Mela> mele = new ArrayList<Mela>();
		prendiFrutta(mele); //errore, non è possibile fare upcasting tra tipi generici, altrimenti potrei aggiungere una pera ad un ArrayList di mele !!!
		
		//qui avrei una lista di mele con una pera !!
		System.out.println(mele.toString());
	}
	public static <T extends Frutto> void prendiFrutta(ArrayList<T> frutti){
		frutti.add(new Pera());
	}
}

//l'upcasting è invece possibile,a tempo di compilazione
```

## peculiarità
### metodi generici in classi non generiche
posso definire metodi generici (anche in classi non generiche !)
per farlo è necessario **anteporre** il tipo generico tra parentesi angolari **al tipo di ritorno**:
```java
static public <T> void esamina(ArrayList<T> lista){
	for(T o : lista)
		System.out.println(o.toString());
}
```

differenze : 
```java
static public void esamina(ArrayList<Frutto> frutti){}
//non accetta ArrayList<Arancia> !!! non sottoclassi del tipo expected, e come visto prima i tipi generici differiscono sulla base dei oro tipi

static public <T extends Frutto> void esamina(ArrayList<T> frutti){}
//questo è veramente generico !! T accetta qualunque sottotipo della classe Frutto
```

per le classi generiche non vale l’ereditarietà dei tipi generici !!
- ArrayList\<Integer> non è di tipo ArrayList\<Number> o ArrayList\<Object>  !!!
ma rimane comunque il polimorfismo tra classi:
```java
List<Integer> listaDiNumeri = new ArrayList<Integer>();
```
