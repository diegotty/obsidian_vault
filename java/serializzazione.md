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