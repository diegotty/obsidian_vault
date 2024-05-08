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
