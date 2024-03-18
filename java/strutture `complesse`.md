# array
- gli array sono oggetti ! quindi le variabili di array contengono il riferimento all'array(0x00AB0124) (che punta al primo elemento dell'array)
- gli elementi di un array possono essere tipi primitivi oppure oggetti
dichiarazione:
```java
int[] a;
```

creazione senza valori:
```java
a = new int[10];
```

>[!tuff]ogni elemento viene inizializzato con il valore di default (0, false, null)

```java
a = new int[] {5, 4, 3, -2, 96, 59, 32, -235959 }; //posso omettere la dimensione
```

meglio usare una COSTANTE per la definizione di un array !