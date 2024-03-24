## definizione
>[!tuff] è una classe “speciale” !!

le enumerazioni sono allo stesso livello delle classi(non sono campi di una classe)
sono un tipo i cui valori possono essere scelti tra un insieme predefinito di identificatori univoci
- ogni valore corrisponde a una costante
- le costanti enumerative sono implicitamente static
- non è possibile creare un oggetto del tipo enumerato
la dichiarazione di una enumerazione può contenere altre componenti tradizionali:
- costruttori
- campi
- metodi


esempio più facile di enum:
```java
public enum Taglia {
	PICCOLA, MEDIA, GRANDE
}
```
classe:
```java
public abstract class Animale {

protected Taglia taglia;
protected int numZampe;
protected String verso;

public Animale(int numZampe, String verso, Taglia taglia) {
	this.verso = verso;
	this.numZampe = numZampe;
	this.taglia = taglia;
}
```