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

## modificare le dimensioni di un array
le dimensioni di un array non possono essere propriamente modificate, ma esiste il metodo:
```java
array = Arrays.copyOf(<array>, int)
```
che crea un nuovo array con gli elementi del primo parametro di lunghezza del secondo parametro 

## metodi con numero di parametri variabile
```java
public static double sum(double... valori){ //double...
	//code
}
```
- valori diventa un riferimento ad array ! 
- ci può essere solo una sequenza variabile di parametri
- i parametri normali vanno definiti prima dell’array variabile

# array a 2 dimensioni
```java
String[][] matrice = new String[RIGHE][COLONNE];
System.out.println(matrice[y][x]);
```