le costanti si scrivono in maiuscolo
```java
public final int MAX = 10000000;
```

non usare graffe non necessarie
```java
if (<condizione>){
	return true;
}
else{
	return false
}

if (<condizione>)
	return true;
else;
	return false;
```

se una funzione viene progettata per restituire un valore, 
>[!tuff] restituiscilo ! non stamparlo !!

il 99% dei campi devono essere pubblico(ancora non è ben conosciuto l’1%)

>[!tuff] NON SCRIVERE METODI STATICI CHE INTERAGISCONO CON CAMPI DI ISTANZA

non ti allargare con le righe di codice, java è già abbastanza prolisso. non scrivere libri se non necessario

non scrivere classi insensate
```java
public class Segmento{
private Punto startPoint;
private 

}
```