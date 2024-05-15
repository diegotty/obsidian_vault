uno stream rappresenta una sequenza di elementi su cui possono essere effettuate una o più operazioni.
supporta operazioni sequenziali e parallele !!

## creazione di uno Stream
lo stream viene creato a partire da una sorgente di dati, ad esempio una java.util.Collection, ma al contrario delle Collection, uno stream non memorizza ne modifica i dati della sorgente, ma opera su essi
```java
```
## oprerazioni intermedie e terminali
- operazioni intermedie: restituiscono un altro stream su cui continuare a lavorare
- operazioni terminali: restituiscono il tipo atteso. 
una volta che uno stream è stato cnsu