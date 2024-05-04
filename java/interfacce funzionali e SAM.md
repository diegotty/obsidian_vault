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