uno stream rappresenta una sequenza di elementi su cui possono essere effettuate una o più operazioni.
supporta operazioni sequenziali e parallele !!

>[!tuff] Stream è un’interfaccia !!

## creazione di uno Stream
lo stream viene creato a partire da una sorgente di dati, ad esempio una java.util.Collection, ma al contrario delle Collection, uno stream non memorizza ne modifica i dati della sorgente, ma opera su essi
- 
```java
//direttamente dai dati
Stream.of(6,5,3,67,3,123,4);

//da una collection, usando i metodi stream() o parallelStream()
ArrayList<Integer> l1 = new ArrayList<>(42,67,59,23532,3);
l1.stream(); //restituisce un nuovo stream sequenziale
l1.parallelStream(); // restituisce un nuovo stream parallelo se possibile (altrimenti restituisce uno stream sequenziale)

//da un array
int[] arr = new int[] {10,5,6,32,46421};
Arrays.stream(arr);

//da un file
BufferedReader.lines()
Files.lines(path);
```

### stream di primitivi
poichè Stream opera su oggetti, esisono versioni ottimizzate per lavorare con 3 tipi primitivi:
- IntStream
- DoubleStream
- LongStream
tutte queste interfacce estendono l’interfaccia di base BaseStream
# oprerazioni 
![[Pasted image 20240515110119.png]]
## op intermedie
restituiscono un altro stream su cui continuare a lavorare
- operazioni terminali: restituiscono il tipo atteso. 
una volta che uno stream è stato consumato (finendo con l’operazione terminale), non può essere riutilizzato !

### filter
```java
Stream<T> filter(Predicate<? super T> predicate)
//accetta un Predicate per filtrare gli elementi dello stream
Predicate<String> startsWithJ = s -> s.startsWith("J");
List<String> l = Arrays.asList("java", "guava", "jam", "gum");

l.stream().filter(startsWithJ).forEach(s -> System.out::print("starts with j: " + s));
```


## op terminali

### min e max
```java
Optional<T> max/min(Comparator<? super T> comparator) 
//restituisce il massimo/minimo elemento all'interno dello stream
List<Integer> p = Arrays.asList(2,3,4,5,6,7,8);
Optional<Integer> max = p.stream().max(Integer::compare);

System.out.println(max.orElse(-1));
```

### forEach
```java
void forEach(Consumer<? super T> action);
//prende in input un Consumer e lo applca ad ogni elemento dello stream
List<Integer> l = Arrays.asList(4,6,5,3,2,343,53);
l.stream.filter(x-> x > 10).forEach(System.out::println);
```

### count
```java
long count();
//restituisce il numero(long) di elementi nello stream
long startsWithA = l.stream.filter(s-> s.startsWith("a").count());

long numberOfLines = Files.lines(Paths.get("yourfile.txt")).count()
```
# comportamento
gli stream adottano una **lazy behavior**: le operazioni intermedie non vengono eseguite immediatamente, ma solo quando si richiede l’esecuzione di un’operazione terminale !
## op stateful e stateless
stateless: l’ elaborazione dei vari elementi può procedere in modo indipendente
stateful: l’elaborazione di un elemento potrebbe dipendere da quella di altri elementi !

cioò detta l’ordine in cui devono essere eseguite le istruzioni in quanto la JVM decide l’ordine delle operazioni intermedie (o almeno ci prova)
- se lo stream ha solo operazioni stateless, la JVM può eseguire le operazioni in qualsiasi ordine (e quindi può ottimizare di più)
- se invece ci sono operazioni stateful, è più difficile trovare un ordine per ottimizzare la memoria, performance etc.

## stile dichiarativo
- lo stream permette di utilizzare uno stile dichiarativo (dichiariamo le operazioni da effetturare, la ma JVM decide l’ordine)
- mentre la collection impone l''utilizzo di uno stile imperativo (dichiarlo le operazioni da effettuare e l’ordine in cui effettuarle, passo per passo)
lo stream si focalizza sulle operazioni di alto livello da eseguire, eventualmente anche in parallelo, **senza specificare come verranno eseguite**
uno stream è quindi: dichiarativo, componibile, e parallelizzabile