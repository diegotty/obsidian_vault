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
// usando l'interfaccia Iterabile, non potrò avere la prima permutazione, (4,4)
```