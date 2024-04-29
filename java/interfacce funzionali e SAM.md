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