molte + forme
xke ?
vogliamo utilizzare facilmente e senza differenziazioni formali oggetti diversi, ma con caratteristiche comuni
![[Pasted image 20240331221332.png|450]]
```java
Animale a = new Gatto();
a.emettiVerso();
a = new Chihuahua();
a.emettiVerso()
```
`a` polimorfa da gatto ad chihuahua(2 sottoclassi di animale)
`a` è, di fatto, un riferimento ad un animale
la selezione del metodo da chiamare avviene in base all’effettivo tipo dell’oggetto riferito dalla variabile
posso però solo chiamare metodi dichiarati in animale o ereditati da gatto(anche se astratti), non metodi dichiarati in gatto

# binding
il binding è il processo di associare la variabile al suo tipo
## binding statico
il compilatore, senza eseguire il programma, stabilisce già i tipi delle variabili in maniera statica (non solo il tipo delle variabili statiche lol)
## binding dinamico
fatto in Java dalla JVM, serve a stabilire (quando viene usato il polimorfismo), quale metodo chiamare (guarda emettiVerso() in esempio). 
>[!tuff] viene chiamato il metodo implementato, ereditato, o assorbito nella classe di cui chiamo il costruttore

è buona pratica implementare (e fare l’override) del metodo toString()

per utilizzare un metodo della superclasse basta usare `super`
```java
@Override
public String toString(){
	String striscia = getStriscia();
	return striscia + " " + super.toString() + " " + striscia;
}
```

## instanceof
l’operatore instanceof, applicato a un oggetto e a un nome di classe, restituisce **true** se l’oggetto è un tipo o sottotipo di quella classe

## casting
### upcasting
posso sempre convertire senza  un cast esplicito un sottotipo a un supertipo (upcasting). devo però dichiarare una nuova variabile
```java
ImpiegatoStipendiato is1 = new ImpiegatoStipendiato("mario"), "imp1", 1500);
Impiegato i = is1;
```

### downcasting
può essere invece necessario convertire un supertipo a un sottotipo (downcasting) e ciò richiede un cast esplicito 
```java
ImpiegatoStipendiato is2 = (ImpiegatoStipendiato)i;
```
>[!tuff] in questo modo non sto creando un nuovo oggetto !! sto giocando con i riferimenti e i vari livelli dell’oggetto

