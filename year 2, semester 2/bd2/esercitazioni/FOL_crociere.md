---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-25T09:15
completed: false
---
Modellare, mediante formule in logica del primo ordine, le seguenti affermazioni, dopo aver progettato opportuni insiemi di simboli di predicato e/o funzione:
1. Ogni crociera viene effettuata utilizzando una nave
2. Ogni crociera segue un itinerario
3. Gli itinerari hanno un nome univoco
4. Ogni crociera tocca un certo insieme non vuoto di destinazioni
5. Le crociere si dividono in crociere di luna di miele e crociere per famiglia.
Per ogni formula, determinare una interpretazione che sia un suo modello.

$V=\{\}, F= \{\}, P=\{crociera/1, nave/1, usa/1\}$
-  Ogni crociera viene effettuata utilizzando una nave (una e una sola nave)
$V=\{\}, F= \{\}, P=\{crociera/1, nave/1, usa/1\}$
FORALL c (
	crociera(c) →
	 EXISTS i ( nave(i) AND usa(c, i) ) AND NOT( 
		 EXISTS i2 ( NOT( i2 = i) AND usa(c,i) )  
	 )
)

oppure
$V=\{\}, F= \{usa/1\}, P=\{crociera/1, nave/1\}$
FORALL c (
	crociera(c) →
	EXISTS i (
		nave(i) AND usa(c)=n
	)
)
in quanto non esiste 
-  Ogni crociera segue un itinerario (un solo itinerario)
-  Gli itinerari hanno un nome univoco
-  Ogni crociera tocca un certo insieme non vuoto di destinazioni
-  Le crociere si dividono in crociere di luna di miele e crociere per famiglia.