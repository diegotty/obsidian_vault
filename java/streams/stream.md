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
List = new ArrayList
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

## op terminali

## comportamento
gli stream adottano una **lazy behavior**: le operazioni intermedie non vengono eseguite immediatamente, ma solo quando si richiede l’esecuzione di un’operazione terminale !
### op stateful e stateless
stateless: l’ elaborazione dei vari elementi può procedere in modo indipendente
stateful: l’elaborazione di un elemento potrebbe dipendere da quella di altri elementi !

cioò detta l’ordine in cui devono essere eseguite le istruzioni in quanto la JVM decide l’ordine delle operazioni intermedie (o almeno ci prova)
- se lo stream ha solo operazioni stateless, la JVM può eseguire le operazioni in qualsiasi ordine (e quindi può ottimizare di più)
- se invece ci sono operazioni stateful, è più difficile trovare un ordine per ottimizzare la memoria, performance etc.
