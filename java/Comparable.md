per garantire un ordinamento sui tipi utilizzati nelle [[strutture dati]] che si fondando su un ordinamento, è necessario che i tipi implementino un’interfaccia speciale, chiamata `Comparable<T>`

| Metodo               | Descrizione                                                                             |
| :------------------- | :-------------------------------------------------------------------------------------- |
| `int compareTo(T o)` | Confronta se stesso con l’oggetto o (restituendo 0 se uguali, -1 se ≤ o, +1 altrimenti) |