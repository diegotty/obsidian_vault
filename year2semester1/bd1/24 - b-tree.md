---
created: 2024-12-13
related to: 
updated: 2024-12-13T17:13
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
l’altezza minima del b-tree è ottenuta quando tutti i blocchi sono pieni, mentre l’altezza massima è ottenuta quando ho il minimo numero di record per ogni blocco.
- se i blocchi, però, sono completamente pieni, un inserimento può richiedere una modifica dell’indice ad ogni livello e in ultima ipotesi può far crescere l’altezza dell’albero di un livello !!! (that would suck, man)
>[!example] esempio catastrofico di inserimento
 si vuole inserire il record con chiave 40 in questo b-tree
 ![[Pasted image 20241213170159.png]]
 per forza di cose, arriviamo a questa forma del b-tree:
 ![[Pasted image 20241213170228.png]]

qual’è il valore massimo $k$ che può assumere l’altezza $h$ ?
$\log_{d}\left( \frac{N}{e} \right)$
## inserimento
l’inserimento costa:
- $h+1$ (costa di una ricerca per ricercare il blocco in cui deve essere inserito il record)
- +1 accesso per riscrivere il blocco del file principale
- altrimenti, se nel blocco **non c’è** spazio sufficiente per inserire il record: $+s$ accessi ($s≤2h+1$) 
	- nel caso peggiore, per ogni livello dobbiamo sdoppiare un blocco: il che vuol dire  effettuare 1 accesso sul blocco presente, e 1 sul blocco nuovo richiesto(entrambi gli accessi in scrittura), più 1 alla fine per la nuova radice