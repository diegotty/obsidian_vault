le interfacce sono uno strumento che Java mette a disposizione per consentire a più classi di fornire e implementare un insieme di metodi comuni
esse definiscono e standardizzano l’interazione fra oggetti tramite un insieme limitato di operazioni
>[!tuff] LE INTERFACCE PERMETTONO DI MODELLARE COMPORTAMENTI COMUNI A CLASSI CHE NON SONO NECESSARIAMENTE IN RELAZIONE GERARCHICA !!(is-a, has-a)
>questa è l’utilità delle interfacce !!


esse specificano soltanto il comportamento (le classi astratte possono definire anche un costruttore) che un certo oggetto deve presentare all’esterno, cioè cosa quell’oggetto può fare. l’implementazione di tali operazioni, cioè come queste vengono tradotte e realizzate, rimane invece non definito
>[!tuff] no costruttori, no implementazioni. le interfacce sono classi astratte al 100% !!!

### perchè esistono i metodi di default e i metodi statici nelle interfacce
i metodi di default sono dovuti all’estensione di interfacce vecchie con nuovi metodi senza rompere il contratto con il codice che utilizza le versioni precedenti

i metodi statici non godono del polimorfismo, sono metodi di utilità non associati alle singole istanze(infatti non interagiscono con campi di classe)
## dichiarazione di un’interfaccia
un’interfaccia è una classe che può contenere soltanto:
- costanti
- metodi astratti
- da java8: metodi di default e metodi statici
- da java9: metodi privati tipicamente da invocare in metodi di default
tutti i metodi dichiarati in un’interfaccia sono implicitamente **public abstact**
tutti i campi dichiarati in un’interfaccia sono implicitamente **public static final**
>[!tuff] tranne per i metodi di default o statici, non è possibile specificare alcun dettaglio implementativo !


## esempio: Iterabile
ci sono molte classi di natura diversa che rappresentano sequenze di elementi, tuttavia le sequenze hanno qualcosa in comune: è possibile iterare sui loro elementi

```java
public interface Iterabile {
	boolean hasNext();
	Object next();
	void reset();
}
```

ciascuna classe implementerà i metodi a suo modo:

```java
public class MyIntegerArray implements Iterabile {
	private Integer[] array;
	private int k = 0;
	
	public MyIntegerArray(Integer[] array) {
		this.array = array;
	}
	
	@Override
	public boolean hasNext() { return k < array.length; }
	
	@Override
	public Object next() { return array[k++]; }
	
	@Override
	public void reset() { k=0; }
}


public class MyString implements Iterabile {
	private String s;
	private int k = 0;
	
	public MyString(String s) {
		this.s = s;
	}
	
	@Override
	public boolean hasNext() { return k < array.length; }
	
	@Override
	public Object next() { return s.charAt(k++); }
	
	@Override
	public void reset() { k=0; }
}
```
questa implementazione però, non è la soluzione ideal per iterare su una collezione, perchè non ci permette di mantenere contatori multipli sullo stesso oggetto.
```java
MyString s = new MyString("ciao");
while(s.hasNext()){
	char c1 = s.next();
	while(s.hasNext()){
		char c2 = s.next();
		System.out.println(c1+"-" + c2);
	}
}
```
questo problema viene risolto con [[Iterable e Iterator]]

# il contratto
implementare un’interfaccia equivale a firmare un contratto con il compilatore, che stabilisce l’impegno ad implementare tutti i metodi specificati dall’interfaccia o a dichiarare la classe abstract.
ci sono 3 possibilità per una classe che implementa un’interfaccia:
- fornire un’implementazione concreta di tutti i metodi, definendone il corpo
- fornire l’implementazione concreta solo per un sottoinsieme proprio dei metodi dell’interfaccia
- decidere di non fornire alcuna implementazione concreta
>[!tuff] negli ultimi 2 casi però, la classe va dichiarata abstract

### perchè utilizzare un’interfaccia al posto di una classe astratta 
potrebbe essere necessario estendere più di una classe astratta(cosa non possibile). al contrario si possono implementare tutte le intefacce desiderate !!
![[Pasted image 20240422233036.png|500]]
sarebbe un problema implementare questa gerarchia usando solo classi !


![[Pasted image 20240422233133.png]]
usando le interfacce posso attribuire a Forma diversi comportamenti. 

## polimorfismo
nel momento in cui una classe C decide di implementare un’interfaccia I, tra queste due classi si instaura una relazione di tipo is-a, ovvero C è di tipo I (comportamento simile all’ereditarietà) quindi anche per le intefacce valgono le regole del polimorfismo  
```java
SupportoRiscribile supporto = new Nastro();
supporto.leggi();
```
in questo modo l’oggetto ha un restringimento della visibilità ai soli metodi dell’interfaccia SupportoRiscrivibile (in quanto l’oggetto della classe Nastro è come se fosse di tipo SupportoRiscrivibile)

### interfacce ed enum
posso rendere le enumerazioni estensibili !!
```java
public interface OperatoreBinario{
	double applica(double a, double b);
}

public enum OperatoriDiBase implements OperatoreBinario{
SOMMA {public double applica (double a, double b) { return a+b;}}.
SOTTRAZIONE {public double applica (double a, double b) { return a-b;}}
PRODOTTO {public double applica (double a, double b){ return a*b;}}
DIVISIONE {public double applica (double a, double b) { return a/b;}}
}
```