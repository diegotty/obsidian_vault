## instanceof
l'operatore, applicato a un oggetto e a un nome di classe, restituisce true se l’oggetto è un tipo o sottotipo di quella classe !
```java
Impiegato i1 = new ImpiegatoStipendiato("diego", "0001", 1);
System.out.println(i1 instanceof Impiegato);
//output è true !
```

## casting 
posso sempre convertire, senza un cast esplicito, un sottotipo a un supertipo (**upcasting**)
- per farlo devo dichiarare una nuova variabile
```java
ImpiegatoStipendiato is1 = new ImpiegatoStipendiato("mario", "imp1", 1500);
```
può essere invece necessario convertire un supertipo a un sottotipo (**downcasting**), ciò richieste un cast esplicito !
```java
ImpiegatoStipendiato is2 = (ImpiegatoStipendiato)i;
```
>[!tuff] NON STO CREANDO UN NUOVO OGGETTO !
>sto giocando con i riferimenti e i vari livelli dell’oggetto

