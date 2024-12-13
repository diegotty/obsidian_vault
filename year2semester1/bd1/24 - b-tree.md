---
created: 2024-12-13
related to: 
updated: 2024-12-13T16:58
---
# b-tree
anche in questo caso, si lavora su dati ordinabili per chiave univoca.
il b-tree nasce dalla generalizzazione della struttura di indice, creando più di livelli del file indice, in modo da poter accedere al file principale attraverso una gerarchia di indici. l’indice a livello più alto nella gerarchia, la **radice**, è **costituito da un unico blocco**, e quindi può risiedere in memoria principale durante l’utilizzo del file.

ogni blocco di un file indice è costituito di record contenenti una coppia $(v,b)$ dove $v$ è il valore della chiave del primo record della porzione del file principale che è accessibile attraverso il puntatore $b$
- $b$ può essere un puntatore ad un blocco del file indice a livello immediatamente più basso, oppure ad un blocco nel file principale
- il primo record indice di ogni blocco indice contiene solo un puntatore ad un blocco (le cui chiavi sono minori di quelle del blocco puntato dal secondo record indice)
- un blocco del file principale è memorizzato come quello di un ISAM
- ogni blocco di un b-tree (indice o file principale), deve essere **pieno almeno al 50%** (quindi per metà della taglia)
	- la radice non ha questo vincolo, l’importante è che sia in un unico blocco

>[!info] rappresentazione b-tree
![[Pasted image 20241213114903.png]]
> questo disegno è fatto proprio con i piedi, capiamo:
> - il primo quadrato di ogni blocco è per il puntatore
> - ogni altro quadrato fa parte di un rettangolo (di lunghezza 2 quadrati), che infatti comprende valore e puntatore.
> - anche questa spiegazione è scritta con i piedi però vabbè

>[!important] ogni record di un file indice, ha una chaive che ricopre quelle del sottoalbero che parte dal blocco puntato
![[Pasted image 20241213115342.png]]
## ricerca
durante la ricerca di un record con un dato valore della chiave, si accede agli indici a partire da quello più alto, e si segue il puntatore del record con la chiave che copre il valore di ricerca, findo ad arrivare ad un unico blocco, del file principale.
per la ricerca sono necessari $h+1$ accessi, dove $h$ è **l’altezza dell’albero**

### altezza di un b-tree
l’altezza minima del b-tree è ottenuta quando tutti i blocchi sono 