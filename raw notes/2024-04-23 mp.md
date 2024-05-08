# more interfacce funzionali built-in
### predicate 
funzione a valori booleani, a un solo argomento generico T
- si usa una lambda function 
```java
Predicate<String> predicate2 = s -> s.startsWith("f");
Predicate<Boolean> nonNull = Objects::nonNull;
```
si possono creare predicati composti con i metodi di predicate(or(), and(), negate() )
### function
funzione a un argomento di tipo T e un tipo di ritorno R entrambi generici
```java
Function<String, Integer> toInteer = Integer::valueOf;
Integer k = toInteger.apply("123");
```
metodi: 
- andThen()
- apply()
- compose()
- identity()
### supplier
funzione senza argomenti in input
```java
Supplier<Person> personSupplier = Person::newPersonSupplier.get(); // new Person()
```

### consumer
funzione con un argomento di tipo generico T e nessun tipo di ritorno
//slide 57, 58

## for each su java.util.Collection
le collection sono ora dotate di un metodo forEach che prende in input un’interfaccia Consumer  // slide 59

## coda 
- fifo
- esiste intefaccia Queue(LinkedList implementa l’intefaccia Queue)
# stack
- lifo
- esiste un’implementazione standard mediante la classe Stack (implementa l’intefaccia List)

## come scegliere un contenitorie
come voglio accedere agli elementi ?
quali sono i tipi degli elementi o tipi di chiavi e valori ?
l’ordine degli elementi o delle chiavi è importante ?
per una collezione non mappa, quali operazioni devono essere veloci ?
se usate rappresentazioni ad albero(TreeSet e TreeMap), serve che si implementi Comparable ?

//alberi

# eccezioni
- le eccezioni rappresentano un meccanismo utile a notificare e gestire gli errori
- un’eccezione indica che durante l’esecuzione si è verificato un errore 
- un eccezione indica un comportamento anomalo, che si discosta dalla normale esecuzione
- la gestione delle eccezioni porta a codice più robusto e sicuro

eccezioni notevoli:

| eccezione                 | descrizione |
| ------------------------- | ----------- |
| IndexOutOfBoundsException |             |
| ClassCastException        |             |
| ArithmeticException       |             |
| lupo gay                  |             |
| ParseException            |             |
| IOError e IOException     |             |
| IllegalArgumentException  |             |
| NumberFormatException     |             |

```java
try{
	svolgi compito 1
	svolgi compito 2
	svolgi compito 3
	svolgi compito 4
}
catch(ExceptionType1 e1){}
catch(ExceptionType2 e2){}
catch(ExceptionType3 e3){}
catch(ExceptionType4 e4){} // l'ordine in cui scrivo le eccezioni conta !!
finally{}

```

l’ordine dei catch conta !!

//slide 19: dato che NumberFormatException è sottotipo di IllegalArgumentException e posizionata in ordine dopo IllegalArgumentException, non verrà mai catchata.

si possono lanciare eccezioni dentro il blocco try: usando la parola chiave `throw`
```java
try{
	if(condizione) throw new Eccezione1();
	else throw new Eccezione2();
}
catch(Eccezione1|Eccezione2 e){
	//gestione di 2 casi in un unico blocco
}
```

quando viene eseguito finally ?

## politica catch-or-declare
una volta solevata un’eccezione, possiamo:
- ignorare l’eccezione e propagarla al metodo chiamante, aggiungendo però all’intestazione del metodo la clausola `throws`, seguito dall’elenco delle eccezioni potenzialmente sollevate (**politica declare**)
- catturare l’eccezione, gestendo la situazione anomala in modo opportuno, prendendo provvedimenti e contromisure per arginare il più possibile la situazione di emergenza (**politica catch**)
>[!tuff] devo perforza usare catch-or-declare !! altrimenti viene emesso un errore. 

questo serve a forzare il programmatore a considerare i problemi legati all’uso di metodi che emettono eccezioni b

printStackTrace()

## creare eccezioni personalizzate
è possibile definire delle eccezioni personalizzate
```java
public class NonToccareLaMiaRobaException extends Exception {

}
```
che significato trasmette IndexOutOfBoundsException ad un utilizzatore di una libreria ? Nessuno !
l’eccezione personalizzata, invece, nasconde i dettagli implementativi e trasmette un significato appropriato al contesto

## il blocco finally
viene eseguito A PRESCINDERE dal sollevamento di eccezioni. le istruzioni all’interno del blocco vengono sempre eseguite(perfino se nel blocco try-catch vi è un return, un break, o un continue)
>[!tuff] un return in finally sovrascrive un return nel try !!!

## throwable
la classe che implementa il concetto delle eccezioni è Throwable, che estende direttamente la classe Object. gli oggetti di tipo Throwable sono gli unici oggetti che è possibile utilizzare con il meccanismo delle eccezioni

//slide 37
## Exception
- eccezioni interne alla JVM, legate ad errori nella logica del programma
## Error
- cattura l’idea di condizione eccezionale irrecuperabile

## checked e unchecked



mvc
model view controller
view: interfaccia Jframe
Observer Observable (tipo observer di minecraft), quando cambia notifica altre cose
il modello non ha riferimenti verso controller o viewer
niente di inerente alla gui deve stare in model
agli actionlistener vanno definiti nel controller
il main:
crea un modello, crea una view, metti la view come osservatrice del modello, e eventualmente ha anche il gameloop(richiede aggiornamento del modello, Thread.sleep(val) xke il thread del main deve dare il tempo alla view per disegnare) 
view implementa observer ? model implementa observable (comunica i cambiamenti del model)? e quindi view capta i cambiamentip
obs permette i messaggi tra observer e observable
