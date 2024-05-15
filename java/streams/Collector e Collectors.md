# collector
```java
public interface Collector<T,A,R>
//T - tipo di input degli elementi da ridurre
//A - il tipo dell'operazione di riduzione (???) spesso nascosto come dettaglio implementativo
//R - il tipo del risultato dell'operazione di riduzione
```

# collectors
classe che implementa varie operazioni di riduzione utili, come accumulare elementi in collezioni, o riassumere elementi in base a vari criteri, etc
## metodi
- `counting()`- restituisce il numero di elementi nello stream 
- 