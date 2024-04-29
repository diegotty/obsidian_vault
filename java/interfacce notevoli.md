
| Interfaccia      | Descrizione                                                                                                                                                                                                                                                                                          |
| ---------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Comparable**   | Impone un ordinamento naturale degli oggetti tramite il metodo: int `compareTo(T b)`, che restituisce un valore >, = o < 0 se l’oggetto è rispettivamente maggiore, uguale o minore di b (T è un tipo generico; può essere di qualsiasi tipo)                                                        |
| **Cloneable**    | Le classi che implementano quest’interfaccia dichiarano al metodo clone() di Object che è legale effettuare una copia campo-a-campo delle istanze della classe<br><br>Il metodo `clone()` invocato su oggetti di classe che non implementano Cloneable solleva una CloneNotSupportedException        |
| **Serializable** | Quest’interfaccia non possiede metodi o campi e serve soltanto ad identificare il fatto che l’oggetto è serializzabile cioè memorizzabile su un certo supporto. Viene ad esempio utilizzato se si vuole fare un “freeze” di una variabile in memoria per poi essere recuperata in un secondo momento |


## runnable
l’interfaccia runnable permette di 