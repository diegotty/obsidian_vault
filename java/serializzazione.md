supponiamo di avere un’informazione da comunicare ad un altro dispositivo. possiamo serializzare l’oggetto e trasmetterlo. in modo tale, esso può essere letto e utilizzato da altri compilatori
```java
public void salva(String filename){
	try{
		FileOutputStream fos = new FileOutputStream(filename);
		ObjectOutputStream os = new ObjectOutputStream(fos);
		os.writeObject(nome);
		os.writeObject(valore);
		os.close();
	}
}
```

per essere serializzabile, una classe deve implementare l’interfaccia Serializable
- Serializable è un’interfaccia senza metodi, è una sorta di marcatore che indica che la classe ha una determinata caratteristica 
quando si serializza un oggetto, tutti gli oggetti variabili d’istanza dell’oggetto originale vengono (e quindi devono essere serializzabili) serializzati
- tutti gli oggetti che sono variabili d’istanza dell’oggetto variabile d’istanza dell’oggetto originale vengono serializzati
- …etc…
se un solo campo non è serializzabile, l’oggetto non è serializzabile
- tuttavia è possibile rendere 