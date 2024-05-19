# collector
```java
public interface Collector<T,A,R>
//T - tipo di input degli elementi da ridurre
//A - il tipo dell'operazione di riduzione (???) spesso nascosto come dettaglio implementativo
//R - il tipo del risultato dell'operazione di riduzione
```
## creazione di un collector
si può creare un collector con il metodo `Collector.of`
```java

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
- la funzione per scegliere la chiave con cui mappare gli oggetti
- la funzione per scegliere il valore con cui mappare gli oggetti
- **opzionale1**: permette di definire, tramite una funzione, il caso in cui un oggetto ha la stessa chiave di un altro oggetto già inserito nella mappa.
- **opzionale2**: il supplier che crea la mappa (per scegliere che tipo di mappa ?)
```java
Map<Integer, String> map = persons.stream().collect(Collectors.toMap(
Person::getAge, Person::getName, (name1, name2) -> name1 + ";" + name2 ));
```

### raggruppamento di elementi
- `groupingBy(lambda che mappa gli elementi di tipo T in bucket rappresentati da oggetti di qualche altro tipo S)` - restituisce una Map\<S, List\<T>> (lo uso per creare una mappa chiave:Lista)
- `groupingBy(lambda, downStreamCollector)` - il parametro downStreamCollector serve per groupare, ma in chiave:collectionSpecificata e non chiave:Lista
- `mapping()` - è un modo di specificare il downStreamCollector, allo stesso tempo “modificando” lo stream !
```java
Map<City, Set<String>> peopleSurnamesByCity = people.stream().collect(
groupingBy(Person::getCity, mapping(Person::getLastName, toSet()));
//sto specificando che nel set ci dovranno essere sono i cognomi !!
```