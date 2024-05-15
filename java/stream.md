## java.util.Optional
è un contenitore di un riferimento, che potrebbe essere o non essere **null**
un metodo può restituire un Optional invece di restituire un riferimento potenzialmente null ! infatti serve ad evitare i NullPointerException

```java
//creazione di un Optional vuoto
Optional.empty()
//creazione di un Optional non nullo
Optional.of("ayooooo");
//creazione di un riferimento che può essere nullo
Optional<String> optional = Optional.ofNullable(s); //in questo caso ha senso usare Optional perchè, se vado ad usare s ed è veramente null, raise-erei una exception, mentre usando Optional me lo posso gestire senza eccezioni
//controllo della presenza di un valore non null:
Optional.empty().isPresent() // == false;
OPtional.of("bum bum ghigno"); // == true
```