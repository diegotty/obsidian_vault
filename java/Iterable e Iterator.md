queste 2 interfacce disaccoppiano l’oggetto su cui iterare dall’oggetto che tiene la posizione dell’iteratore
esempio: voglio creare un programma che crea tutte le permutazioni di un array
```java
MyIntegerArray arr = new MyIntegerArray({4,5,6,6,7,8,4,21,34,5});
int i = 0;
int j = 0;
while(arr.hasNext()){
	System.out.println(arr.curr())
	while(arr.hasNext()){
		print(arr.curr());
		arr.next();
	}
	arr.next();
	}
// usando l'interfaccia Iterabile, non potrò avere la permutazione, (5,4)
```

`java.lang.Iterable`

| Modifier and Type | Method and Description                                                                |
| ----------------- | ------------------------------------------------------------------------------------- |
| `Iterator<T>`     | `iterator()`<br>Restituisce ogni volta che lo chiamo una nuova istanza dell’iteratore |

`java.lang.Iterator`
chi implementa Iterable restituisce un Iterator sull’oggetto-collezione

| Modifier and Type | Method and Description                                        |
| ----------------- | ------------------------------------------------------------- |
| `boolean`         | `hasNext()`<br>Ritorna true se l’iterazione ha altri elementi |
| `E`               | `next()`<br>Ritorna il prossimo elemento dell’iterazione      |
| `void`            | `remove()`<br>                                                |

# il contratto
implementare un’interfaccia equivale a firmare un contratto con il compilatore, che stabilisce l’impegno ad implementare tutti i metodi specificati dall’interfaccia o a dichiarare la classe abstract.
ci sono 3 possibilità per una classe che implementa un’interfaccia:
- fornire un’implementazione concreta di tutti i metodi, definendone il corpo
- fornire l’implementazione concreta solo per un sottoinsieme proprio dei metodi dell’interfaccia
- decidere di non fornire alcuna implementazione concreta
>[!tuff] negli ultimi