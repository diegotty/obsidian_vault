è importante tenere a mente la differenza tra [valori primitivi][] e [oggetti][]
## valori primitivi
memoria allocata automaticamente a tempo di compilazione
## oggetti
memoria allocata durante l'esecuzione del programma (operatore new)
>[!tuff] le stringhe sono oggetti !!

al momento della creazione di un oggetto i campi di una classe sono inizializzati automaticamente
![[Schermata 2024-03-08 alle 16.59.08.png]]
## riferimenti
i riferimenti sono indirizzi di memoria di cui non conosciamo il valore numerico. quindi gli oggetti non sono mai memorizzati direttamente nelle variabili, ma mediante il loro riferimento
***	
# anatomia della memoria
esistono 3 tipi di memoria:
## stack
area di memoria in cui vengono allocate le variabili locali
## heap 
aree di memoria allocate per la creazione dinamica
## metaspace
area di memoria in cui vengono allocati i campi [static][], prima di qualsiasi oggetto della classe
![[Schermata 2024-03-08 alle 17.09.22.png|400]]


## memory snapshot
per prima cosa si allocano le variabili statiche.
>[!tuff] i campi delle classi hanno un meccanismo di inizializzazione automatica !

si analizza il main
viene creato un frame nello stack per il main
i parametri vengono allocati (anche se vuoti !!)
il puntatore delle variabili vengono creati nel frame dello scope 
se l'oggetto non ha campi di oggetto, viene creato ma il puntatore non punta a niente (?)
alla nuova chiamata in un metodo viene creato un un frame nello stack
variabili dichiarate nei cicli diventano variabili nello stack(che puntano alla memoria allocata nella heap), quando finisce il for vengono eliminate
variabili dichiarate e non inizializzate diventano riferimenti solo nello stack in questo modo:
```java
int g;

g undef //non punta a nulla !
```
le variabili dichiarate e inizializzate a null diventano riferimenti solo nello stack in questo modo:
```java
String s = null;

s null // non punta a nulla ! ma è ok
