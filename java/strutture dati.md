caratteristiche che distinguono le strutture dati:
- è necessario mantenere un ordine ? 
- gli oggetti nella struttura possono ripetersi ?
- è necessario possedere una “chiave” per accedere a uno specifico oggetto ?
# Collection
- le collezioni in java sono rese disponibili mediante il Java Collection Framework
- sono strutture dati già pronte all’uso, con interfacce e algoritmi per manipolarle
- contengono e strutturano riferimenti ad altri oggetti (tipicamente tutti dello stesso tipo)
alcune interfacce del framework:

| Interfaccia | Descrizione                                             |
| ----------- | ------------------------------------------------------- |
| Collection  | L’interfaccia alla radice della gerarchia di collezioni |
| Set         | una collezione senza duplicati                          |
| List        | una collezione ordinata che può contenere duplicati     |
| Map         | associa coppie (chiave, valore) senza chiavi duplicate  |
| Queue       | Una collezione FIFO, che modella una coda               |
## la gerarchia
![[Pasted image 20240429205017.png|600]]
## interazione su collezioni
### iterazione esterna:
- mediante gli Iterator (vecchio stile)
```java
Iterator<Integer> i = collezione.iterator();
while(i.hasNext()) {      // finché ha un successivo
	int k = i.next();     // ottieni prossimo elemento
	System.out.println(k);
}
```
- mediante il costrutto for each (per ogni elemento della collezione )
```java
for (Integer k : collezione)
	System.out.println(k);
```
- mediante gli **indici**(accesso elemento per elemento, + controllo)
```java
for (int j=0; j<collezione.size(); j++) {
	int k = collezione.get(j);
	System.out.println(k);
}
```
### iterazione interna:
- mediante il metodo `Iterable.forEach` che permette l’iterazione su qualsiasi collezione senza specificare come effettuare l’iterazione(utilizza il polimorfismo, chiamerà il forEach della classe specificata)
	- il metodo forEach prende in input un Consumer, che è un’interfaccia funzionale con un solo metodo
```java
public interface Consumer{
	void accept(T t);
}
//esempio iterazione interna
List<Integer> I = List.of(4,8,15,16,23,42);
I.forEach(x -> System.out.println(x));
//il metodo forEach prende un Consumer
```