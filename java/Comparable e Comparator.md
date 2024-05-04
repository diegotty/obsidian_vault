per garantire un ordinamento sui tipi utilizzati nelle [[strutture dati]] che si fondando su un ordinamento, è necessario che i tipi implementino un’interfaccia speciale, chiamata `Comparable<T>`

| Metodo               | Descrizione                                                                             |
| :------------------- | :-------------------------------------------------------------------------------------- |
| `int compareTo(T o)` | Confronta se stesso con l’oggetto o (restituendo 0 se uguali, -1 se ≤ o, +1 altrimenti) |

in alternativa, per ordinare in modo diverso gli elementi di un certo oggetto, si può implementare l’interfaccia `Comparator<T>` e passarne un’istanza in input al costruttore delle strutture dati 

| Tipo      | Metodo                                                                                        |
| --------- | --------------------------------------------------------------------------------------------- |
| `int`     | **`compare(T o1, T o2)`**<br>Compares its two arguments for order                             |
| `boolean` | **`equals(Object obj)`**<br>Indicates whether some other object is “equal to” this comparator |
