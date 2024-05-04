- disponibili da Java 8 grazie all’annotazione `@FunctionalInterface`
- l’annotazione garantisce che l’interfaccia sia dotata esattamente di un solo metodo astratto
```java
@FunctionalInterface
public interface Runnable{
	void run();
}
```
nel caso in cui ci fosse un’interfaccia composta da 2 metodi, di cui uno solo astratto, essa viene comunque considerata una `@FunctionalInterface` ! quando utilizzerò una [[classi anonime e lambda function#lambda function|lambda function]]
questa si riferirà in automatico al metodo astratto !

# SAM (single abstract method)
le interfacce funzionali sono di tipo SAM
>[!tuff] a ogni metodo(tipo forEach che prende un Consumer) che accetta un’interfaccia SAM, si può passare un’espressione lambda compatibile con l’unico metodo dell’interfaccia SAM (analogamente per un riferimento a un’interfaccia SAM)

le interfacce funzionali possono essere “espresse” in Lambda !(in qualche modo, per qualche motivo, guarda [[strutture dati#iterazione interna|esempio iterazione interna]], posso passare come argomento una lambda function a un metodo che prende come argomento un’oggeto di una classe che implementa l’interfaccia funzionale.  the bts does all that !! crazy.)
esempio:
![[Pasted image 20240504173222.png]]
```java
List<String> names = Arrays.asList("peter", "anna", "mike", "xenia");
Collections.sort(names, (a,b) -> b.compareTo(a));
//ho passato una lambda function a un metodo che prende in input una classe che implementa Comparator
```


# interfacce funzionali built-in
## Predicate\<T>
| Modifier and Type         | Method                                                                                                                                             |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `default Predicate<T>`    | **`and(Predicate<? super T> other)`**<br>Returns a composed predicate that represents a short-circuiting logical AND of this predicate and another |
| `static <T> Predicate<T>` | **`isEqual(Object targerRef)`**<br>Returns a predicate that tests if two arguments are equal according to `Object.equals(Object, Object)`          |
| `default Predicate<T>`    | **"`negate()`**<br>Returns a predicate that represents the logical negation of this predicate                                                      |
| `default Predicate<T>`    | **`or(Predicate<? super T> other)`**<br>Returns a composed predicate that represents a short-circuiting logical OR of this predicate and another   |
| `boolean`                 | **`test(T t)`**<br>Evaluates this predicate on the given argument                                                                                  |
funzione a valori booleani a un solo argomento generico T
```java
Predicate<String> predicate = s -> s.length() > 0;
predicate.test("foo"); //true
```
si possono creare predicati composti con i metodi di predicate(or(), and(), negate() )

## Function\<T, R>
funzione a un argomento di tipo T e un tipo di ritorno R, entrambi 