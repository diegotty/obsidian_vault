- è possibile definire classi anonime (ovvero senza nome) che implementano un’interfaccia o estendono una classe
- utilizzate esclusivamente per creare un’unica istanza
- utili in determinati contesti (es: per creare un iteratore al volo (???))
```java
TipoDaEstendere unicoRiferimentoAOggetto = new TipoDaEstendere()
{
//codice della classe anonima(implementazione dell'interfaddia o // estensione della classe)
};
```
## perchè è utile ??
è utile se il codice non servirà mai più oltre alla scope in cui viene creata la classe anonima !
```java
public interface formula {
	double calculate(int a);
	default double sqrt(int a) { return Math.sqrt(a);}
}
```

```java
Formula formula = new Formula(){ ///tipo da estendere !!!!!! crazy !!!!!!!!
	@Override
	public double calculate(int a){
		return sqrt(a * 100);
	}
}
```

# lambda function
da Java 8 è possibile specificare funzioni utilizzando una notazione molto compatta: le espressioni lambda.
```java
(tipo nome_param1, ..., tipo nome_paramn) -> { codice della funzione}
```
>[!tuff] tali espressioni creano oggetti anonimi, assegnabili a riferimenti a interfacce funzionali compatibili con l’intestazione (input/output) della funzione creata