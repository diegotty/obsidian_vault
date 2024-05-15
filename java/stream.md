## java.util.Optional
è un contenitore di un riferimento, che potrebbe essere o non essere **null**
un metodo può restituire un Optional invece di restituire un riferimento potenzialmente null ! infatti serve ad evitare i NullPointerException
```java
//creazione di un Optional vuoto
Optional.empty()
//creazione di un Optional non nullo
Optional.of("ayooooo");
//creazione di un riferimento che può essere nullo

```