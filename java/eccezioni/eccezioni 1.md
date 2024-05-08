# eccezioni
- le eccezioni rappresentano un meccanismo utile a notificare e gestire gli errori
- un’eccezione indica che durante l’esecuzione si è verificato un errore 
- un eccezione indica un comportamento anomalo, che si discosta dalla normale esecuzione
- la gestione delle eccezioni porta a codice più robusto e sicuro

## eccezioni notevoli

| Eccezione                    | Descrizione                                                                                                        |
| :--------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| `IndexOutOfBoundsException`  | Accesso ad una posizione non valida di un array o una stringa (<0 o maggiore della sua dimensione)                 |
| `ClassCastException`         | Cast illecito di un oggetto ad una sottoclasse a cui non appartiene<br>Es. `Object x = new Integer(0); (Stringa)x` |
| `ArithmeticException`        | Condizione aritmetica non valida (es. divisione per zero)                                                          |
| `CloneNotSupportedException` | Metodo `clone()` non implementato o errore durante la copia dell’oggetto                                           |
| `ParseException`             | Errore inaspettato durante il parsing                                                                              |
| `IOError` e `IOException`    | Grave errore di input o output                                                                                     |
| `IllegalArgumentException`   | Parametro illegale come input di un metodo                                                                         |
| `NumberFormatException`      | Errore nel formato di un numero (estende la precedente)                                                            |

## cosa si può gestire con le eccezioni ?
si possono gestire:
- errori sincroni, che si verificano a seguito dell’esecuzione di un istruzione
	errori non critici: errori che derivano da condizioni anomale
	- divisione per zero
	- errore di I/O
	- errori durante il parsing
	errori critici o irrecuperabili: errori interni alla JVM
	- conversione di un tipo non consentito
	- accesso ad una variabile riferimento con valore null
	- mancanza di memoria libera
	- riferimento a una classe inesistente

non si possono gestire:
- eventi asincroni
	completamenti nel trasferimento I/O
	ricezione messaggi su rete
	click del mouse
- eventi che accadono parallelamente all’esecuzione e quindi indipendenti dal flusso di controllo
## try-catch

```java
try{
	svolgi compito 1
	svolgi compito 2
	svolgi compito 3
	svolgi compito 4
}
catch(ExceptionType1 e1){}
catch(ExceptionType2 e2){}
catch(ExceptionType3 e3){}
catch(ExceptionType4 e4){} // l'ordine in cui scrivo le eccezioni conta !!
finally{}
```
nell’attuare il processo di cattura, la JVM sceglie **il primo catch compatibile**, tale cioè che il tipo dell’eccezione dichiarata sia lo stesso o un supertipo dell’eccezione lanciata