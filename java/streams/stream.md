uno stream rappresenta una sequenza di elementi su cui possono essere effettuate una o più operazioni.
supporta operazioni sequenziali e parallele !!

>[!tuff] Stream è un’interfaccia !!
>per natura, gli stream sono parallelizzabili

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

### sorted
```java
//restituisce uno stream ordinato senza modificare la collezione da cui è stato creato lo stream
List<String> l = Arrays.asList("da", "ab", "ac", "bb");
l.stream().sorted().forEach(System.out::print);
```

### map
```java
//restituisce un nuovo stream in cui ciascun elemento dello stream originale è convertito in un altro oggetto attraverso la Function passata in input
<R> Stream<R> map(Function<? super T, ? extends R> mapFunction);
//non è PECS
l.stream().map(s -> s.toUpperCase()).forEach(System.out::println);

// il vincolo su R ha senso perchè mi sto dando la possibilità di creare uno stream di ritorno di un sottotipo del secondo tipo generico di R
Function<Integer, Double> func = x -> x * 2;
Stream<Float> s = l.stream().map(func);
```

### distinct
```java
//restituisce un nuovo stream senza ripetizione di elementi (gli elementi sono tutti distinti tra loro)
List<Integer> l = List.of(3,4,5,6,4,2,1,6,3);
List<Integer> distinti = l.stream().map(x -> x*x).distinct().collect(Collectors.toList());
```

### limit
```java
Limita lo stream a k elementi(k long passato in input)
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

long numberOfLines = Files.lines(Paths.get("yourfile.txt")).count();
```

### collect
```java
<R,A> R collect(Collector<? super T, A, R> collectorFunction);
// permette di raccogliere gli elementi dello stream in qualche oggetto (collection, una stringa, un intero)
List<Integer> ivaEsclusa = Arrays.asList(10, 20, 30);
List<Double> l = ivaEsclusa.stream().map(p->p*1.22).collect(Collectors.toList());
```

### reduce
```java
//effettua una riduzione sugli elementi dello stream utilizzando la funzione data in input
T reduce(T identity, BinaryOperator <T> accumulator);
list.stream.reduce(0, (a,b) -> a+b);
list.stream.reduce(0, Integer::sum);
// il primo parametro, l'elemento di identità, serve come "inizio"
//riduce ad un solo numero, la somma di tutti gli elementi dello stream !

```
### reduce (con Optional)
```java
//esiste anche una versione di reduce con solo un parametro, che restituisce un Optional<T>
T reduce(BinaryOperator <T> accumulator);
lista.stream().reduce(Integer::sum);
//viene usato Optional perchè se lo stream è vuoto, non avendo l'elemento di identità, non si sa quale valore restituire
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