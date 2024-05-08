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
è ovviamente meglio rispondere ad un’eccezione con il rimedio specifico e non con uno generale !

si possono lanciare eccezioni dentro il blocco try: usando la parola chiave `throw`
```java
try{
	if(condizione) throw new Eccezione1();
	else throw new Eccezione2();
	// con la parola chiave throw posso lanciare un'eccezione
}
catch(Eccezione1|Eccezione2 e){
	//gestione di 2 casi in un unico blocco
}
```

### flusso in presenza o assenza di eccezioni
se durante l’esecuzione non vengono sollevate eccezioni:
- tutte le istruzioni dentro il try-catch vengono eseguite normalmente
- terminato il blocco try-catch, l’esecuzione riprende dalla prima linea dopo il blocco try-catch
se viene sollevata un’eccezione:
- l’esecuzione del blocco try viene bloccata
- il controllo passa al primo blocco catch compatibile
- l’esecuzione riprende dalla prima linea dopo il blocco try-catch
## politica catch-or-declare
una volta solevata un’eccezione, possiamo:
- ignorare l’eccezione e propagarla al metodo chiamante, aggiungendo però all’intestazione del metodo la clausola `throws`, seguito dall’elenco delle eccezioni potenzialmente sollevate (**politica declare**)
```java
public static void main(String[] args) throws EccezionePazza, EccezioneMatta {
//con throws dichiaro che il metodo può solleverare eccezioni dello stesso tipo (o sottotipo) di quelle elencate dopo throws
//usare throws non è obbligatorio, dipende dal tipo di eccezione
}
```
- catturare l’eccezione, gestendo la situazione anomala in modo opportuno, prendendo provvedimenti e contromisure per arginare il più possibile la situazione di emergenza (**politica catch**)
>[!tuff] devo perforza usare catch-or-declare !! altrimenti viene emesso un errore. 

## stacktrace
quando un’eccezione non viene mai catturata, su schermo viene stampato un “riassunto” associato all’eccezione non catturata, chiamato stacktrace
```java
Exception in thread "main" NonToccareLaMiaRobaException 
at Armadietto.apriArmadietto(Armadietto.java:11)
at Spogliatoio.main(Spogliatoio.java:10)
```
questo messaggio viene generato dal metodo `printStackTrace()`, offerto dalla classe Throwable
per una descrizione sintetica (se prevista o disponibile) della ragione per la quale si è verificata l’eccezione, si può usare il metodo `getMessage()`

## creare eccezioni
è possibile definire eccezioni personalizzate, per conservare la semantica dell’applicazione.
```
```
al momento della creazione si dovrà studiare la natura e lo scopo delle eccezioni e scegliere la super-classe più adeguata

```java
public class NonToccareLaMiaRobaException extends Exception {

}
```
