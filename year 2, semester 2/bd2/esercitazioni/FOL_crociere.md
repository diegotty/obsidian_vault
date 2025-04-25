---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-25T09:00
completed: false
---
>[!info] momento ricapitolazione
>- variabili: oggetti del dominio
>- simboli di predicato: restituiscono `true/false`
>- simboli di funzione: restituiscono un termine 
>- costanti: simboli di funzione di arità 0
>- termini: variabili/ funzioni costanti/ funzionicon termini come argomenti
>- formule: simboli di predicato che hanno come argomenti termini/ formule separate da operatori logici/ formule con quantificatori
>- **valutazione dei termini**: 
>	- **pre-interpretazione**: dominio di interpretazione + corrispondenza dai simboli di funzione a una funzione totale (definita esplicitamente per ogni elemento del dominio) del tipo $D^n \to D$ : la funzione$preI(f)$)
>	- **assegnamento delle variabili**: una funzione $V \to D$ che associa ogni variabile ad un elemento del dominio (**separato dalla pre-interpretazione**)
>	- **valutazione dei termini (complessi)**: funzione di valutazione (basata su un assegnamento e una funzione di pre-interpretazione), definita induttivamente (è di default, pusho dentro le cose)
>- **valutazione delle formule**:
>	- **interpretazione**: 
>	- **valutazione delle formule complesse a partire dalla valutazione delle formule atomiche

Modellare, mediante formule in logica del primo ordine, le seguenti affermazioni, dopo aver progettato opportuni insiemi di simboli di predicato e/o funzione:
1. Ogni crociera viene effettuata utilizzando una nave
2. Ogni crociera segue un itinerario
3. Gli itinerari hanno un nome univoco
4. Ogni crociera tocca un certo insieme non vuoto di destinazioni
5. Le crociere si dividono in crociere di luna di miele e crociere per famiglia.
Per ogni formula, determinare una interpretazione che sia un suo modello.

-  Ogni crociera viene effettuata utilizzando una nave (una e una sola nave)
-  Ogni crociera segue un itinerario (un solo itinerario)
-  Gli itinerari hanno un nome univoco
-  Ogni crociera tocca un certo insieme non vuoto di destinazioni
-  Le crociere si dividono in crociere di luna di miele e crociere per famiglia.