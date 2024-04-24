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

quando voglio rendere una classe iterabile, essa deve implementare Iterable. in questo modo la classe sarà obbligata ad implementare il metodo iterator(), in cui bisognerà creare e ritornare un oggetto di tipo Iterator. 
nel video trovato 