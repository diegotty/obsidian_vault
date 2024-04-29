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

### peculiarità
la parola chiave this si riferisce all’oggetto anonimo
le classi anonime vengono compilate come classi interne


# lambda function
da Java 8 è possibile specificare funzioni utilizzando una notazione molto compatta: le espressioni lambda.
```java
(tipo nome_param1, ..., tipo nome_paramn) -> { codice della funzione}
```
- il tipo dei parametri in input è opzionale, perché si ricava dal contesto dell’interfaccia a cui facciamo riferimento
- le parentesi tonde sono opzionali se in input abbiamo un solo parametro
- le parentesi graffe intorno al codice sono opzionali se è costituito da una sola riga
- non è necessario return, se il codice è dato dall’espressione di ritorno(viene assunto il return, guada esempio sotto)
```java
(a,b) -> a+b;
```
## perchè sono utili ?????
- è da consigliare l’impiego delle espressioni lambda **principalmente** quando il codice si scrive su una sola riga
- in alternativa, si preferisce un’implementazione mediante classe o classe anonima ( o riferimenti a metodi)

>[!tuff] tali espressioni creano oggetti anonimi, assegnabili a riferimenti a interfacce funzionali compatibili con l’intestazione (input/output) della funzione creata

credo quello sopracitato sia il loro quasi unico scopo ? l’essere assegnate a interfacce funzionali !

esempio di Formula con lambda function
```java
Formula formula = a -> Math.sqrt(a*100);
//ho una classe anonima che implementa un'interfaccia funzionale usando una lambda function per implementare il metodo astratto
```

esempio converter 
```java
public interface Converter<F,T>{
	T convert(F from); // parametro di tipo F chiamato from
}
Converter<String, Integer> converter = from -> Integer.valueOf(from); //ha senso...
Integer converted = converter.convert("123"); //non sto creando una nuova istanza di converter, sto utilizzando l'unica istanza per creare un oggetto di tipo Integer ! fun !!!!!!!!!! fuck
```

### peculiarità
la parola chiave this si riferisce all’oggetto della classe che le racchiude
le espressioni lambda vengono compilate come metodi privati invocati dinamicamente