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