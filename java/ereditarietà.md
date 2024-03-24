# ereditarietà
concetto cardine della programmazione orientata agli oggetti !
è una forma di riuso del software in cui una classe è creata:
- assorbendo i membri di una classe esistente
- aggiungendo nuove caratteristiche e migliorando quelle esistenti
![[Pasted image 20240324215637.png]]
cerchio, triangolo e rettangolo cosa hanno in comune ?
dalla risposta hanno deciso di creare la classe forma 
date poi le distinzioni di ognuno, vengono create le sottoclassi cerchio, triangolo, rettangolo
```java
public class Forma{
public void disegna(){}
}
public class Triangolo extends Forma{
	//la classe Triangolo eredita i membri di Forma !
	//in questo caso il metodo disegna() è vuoto
}
```
## che cosa si eredita ?
- ogni sottoclasse estende solo una superclasse
- la sottoclasse eredita campi e metodi d’istanza secondo il livello d’accesso specificato
- la sottoclasse può aggiungere nuovi metodi e campi
- ridefinire i metodi che eredita dalla superclasse(tipicamente non i campi)
- le classi ereditano in maniera transitiva (se a eredita da b, e c eredita da a, c eredita da b)
