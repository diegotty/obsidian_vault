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
### riduzioni a singolo elemento
- `counting()`- restituisce il numero di elementi nello stream 
- `maxBy/minBy(Comparator)` - restituisce un Optional(in verità restituisce un Collector con parametro R che è un Optional) con il massimo/minimo valore
- `summingIng()/summingDouble()`
- `ageragingInt()/averagingDouble()`
- `joining(), joining(separatore), joining(separatore, prefisso, suffisso)` - concatena gli elementi stringa dello stream in un’unica stringa finale (il separatore è di tipo CharSequence)
- `toList(), toSet(), toMap()` - accumulano gli elementi in una lista, un set o una map (non c’è garanzia sul tipo di List, Set o Map) (chissà come fa ….)
- `toCollection(Supplier<C> collectionFactory)` - accumula gli elementi in una collezione a scelta ( da esempio, si passa il riferimento al costruttore (ArrayList::New))
#### toMap
more depth on riduzione a una mappa:
toMap prende fino a 4 argomenti:

### raggruppamento di elementi