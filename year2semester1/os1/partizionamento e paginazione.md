---
created: 2024-10-27
related to: "[[intro allo scheduling]]"
updated: 2024-10-27, 15:35
---
studiamo ora i diversi tipi di strategie usate nel tempo per gestire la memoria
# partizionamento
il partizionamento è il metodo più antico, e ci sarà utile per capire la memoria virtuale (che è l’evoluzione moderna delle tecniche di partizionamento)
## partizionamento fisso uniforme
all’avvio, la memoria principale viene divisa in partizioni di uguale lunghezza
- se un processo ha una dimensione minore o uguale all misura di una partizione, allora può essere caricato in una partizione libera
- il SO può togliere un processo da una partizione (es: nessuno dei processi in RAM è `ready`)
>[!figure] ![[Pasted image 20241023102731.png|100]]
### problemi irrisolti
- un programma potrebbe non entrare in una partizione: tocca al programmatore usare l’overlaying
- uso inefficiente della memoria: ogni programma, anche il più piccolo, occupa un’intera partizione
- frammentazione interna: ogni partizione è divisa in 2: una parte usata e una non usata
## partizionamento fisso variabile
>[!figure]  ![[Pasted image 20241023102903.png|100]]

le partizioni sono decise all’inizio e non cambiano più, (come il parzionamento fisso variabile) ma le partizioni create sono a dimensione incrementale
- nella foto, si possono gestire fino a processi di 16M senza obbligare il programmatore a usare l’overlaying, e per i processi più piccoli ci sono le partizioni più piccole
dato che esisono partizioni di diversa lunghezza, bisogna usare un **algoritmo di posizionamento** che segue delle regole:
- il processo va nella partizione più piccola che può contenerlo, per minimizzare la quantità di spazio sprecato
ciò si può ottenere in 2 modi: con una coda per partizione, o una coda unica 
### problemi irrisolti
- c’è un numero massimo di processi in memoria principale, pari al numero di partizioni deciso inizialmente
- se ci sono molti processi piccoli, la memoria verrà usata in modo inefficiente (sia con partizionamento fisso che fisso variabile)
## partizionamento dinamico
le partizioni variano sia in misura che in quantità, e per ciascun processo viene allocata esattamente la quantità di memoria che serve
usando il partizionamento dinamico, si va in contro a:
- frammentazione esterna: la memoria non usata da nessuno dei processi viene frammentata (guarda esempio)
la frammentazione può essere risolta con la **compattazione**: il SO sposta i processi di modo che siano contigui
- ciò però ha un grande overhead
### algoritmi di posizionamento
inoltre, come nel partizionamento fisso variabile, devo decidere dove mettere ogni processo. ne esistono diversi:
- **algoritmo best-fit**: sceglie il blocco la cui misura è più vicina a quella del processo da posizionare. in questo modo però, lascia dei frammenti molto piccoli, e costringe al SO di fare spesso la compattazione
- **algoritmo first-fit**: scorre la memoria dall’inizio, prende il primo blocco con abbastanza memoria. in questo modo  è molto veloce, ma tende a riempire solo la prima parte della memoria
- **algorimo next-fit**: come il first-fit, ma tiene conto dell’ultima posizione assegnata ad un processo, e partendo da lì la ricerca per una partizione libera.
>[!example] esempio dei tre algoritmi
![[Pasted image 20241025082900.png]]
# buddy system
permette un compromesso tra partizionamento fisso e variabile(ed è ancora usato, in casi particolari !)
sia $2^U$ la dimensione dello user space (memoria principale - spazio OS), ed $s$ la dipensione di un processo da mettere in RAM
si prosegue in questo modo:
la memoria viene dimezzata fino a trovare una $X$ tale che $2^{X-1} < s \leq 2^X$, con $L \leq X \leq U$, in cui $L$ è usato per dare un lower bound (non creare partizioni troppo piccole)
una delle due porzioni dell’ultimo dimezzamento è usta per il processo
quando il processo finisce, se l’altra partizione creata dal dimezzamento è libera, **si può fare la fusione**
>[!example] esempio di buddy system
![[Pasted image 20241025092804.png]]

>[!figure] rappresentazione in albero binario del buddy system
![[Pasted image 20241025092932.png]]
data la natura del buddy system (il dimezzamento ricorsivo), è molto facile rappresentare gli spazi di memoria come un albero binario

# paginazione semplice
**sia la memoria che i processi** vengono partizionati in pezzi di grandezza uguale (e piccola)
- i pezzi di processi (in genere in memoria ausiliaria) sono chiamati **pagine**
- i pezzi di memoria sono chiamati **frame**
ogni pagina, per essere usata, deve essere collocata in un frame, e data la partizione in grandezza uguale, **ogni pagina può essere collocata in qualunque frame**
- pagine contigue possono essere messe in frame distanti
c’è un grande overhead giustificato dal miglioramento di prestazioni
i SO che adottano la paginazione mantengono una tabella delle pagine di ogni processo, che, per ogni pagina del processo, indica in che frame effettivo si trova
- la situazione dei module relativi va modificata: ogni indirizzo di memoria può essere visto come un numero di pagina e un offset al suo interno
quando c’è un process switch, la tabella delle pagine del nuovo processo deve essere ricaricata
>[!example] esempio di paginazione

>[!info] traduzione della paginazione
supponiamo che la dimensione di una pagina dell’esempio sopra sia 100bytes: la RAM sarà di solo 1400 bytes e i processi A,B,C,D richiedono solo 400,300, 400 e 500 bytes rispettivamente
nelle istruzioni dei processi, i riferimenti alla RAM sono relativi all’inizio del processo(quindi, per D, ci saranno solo riferimenti compresi nell’intervallo [0,499])
supponiamo ora che il processo D, in esecuzione, debba eseguire l’istruzione `j 343`(salto al byte 343). ovviamente D non si sta riferendo all’indirizzo 343 della RAM: lì c’è il processo A!
bisogna quindi:
>- capire in quale pagina di D si trova il 343 giusto: basta fare $\frac{343}{100}=3$ , sappiamo quindi che si trova nella pagina 3 di D.
>- controllare, nella tabella delle pagine di D, in che frame si trova la pagina D3: in questo caso corrisponde al frame 11
>- trovare il byte giusto a cui saltare: il frame 11 ha byte da 1100 a 1199: basta fare $343 \text{mod} 100 = 43$: dobbiamo quindi saltare al 44-esimo byte
>- l’indirizzo assoluto giusto è quindi: $11 \times 100 + 43 = 1143$
# segmentaz