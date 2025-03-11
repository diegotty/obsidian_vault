---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-11T10:07
completed: false
---
>[!info] problema
dato un grafo $G$ (diretto o non diretto), ed un suo nodo $u$, vogliamo sapere se da $u$ è possibile raggiungere un ciclo in $G$
![[Pasted image 20250311095011.png]]
# cicli
partiamo da un’idea intuitiva ma sbagliata: visitiamo il grafo, e se nel corso della visita incontriamo un nodo che è già stato visitato, vuol dire che esiste un ciclo: interrompiamo la ricerca e restituiamo `True`
- banalmente, questa idea non funziona per i grafi non diretti: essendo ogni arco bidirezionale, questo algoritmo interpreta ogni arco come un ciclo di lunghezza 2!
- inoltre, è scorretto anche per i grafi diretti !! (che casino). in quanto in un grafo diretto, incontare un nodo già visitato non vuol necessariamente dire che si è in presenza di un ciclo (guarda immagine)
>[!example]- grafo diretto senza ciclo
>![[Pasted image 20250311100214.png]]

## archi verso nodi già visitati
(grafi diretti)
**archi in avanti**:archi che collegano un antenato ad un suo discendente (non suo figlio, ma almeno il grado dopo)
**archi all’indietro**: il contario di un arco in avanti
**archi di attraversamento**: archi 