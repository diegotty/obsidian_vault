---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-11T09:52
completed: false
---
>[!info] problema
dato un grafo $G$ (diretto o non diretto), ed un suo nodo $u$, vogliamo sapere se da $u$ è possibile raggiungere un ciclo in $G$
![[Pasted image 20250311095011.png]]
# cicli
partiamo da un’idea intuitiva ma sbagliata: visitiamo il grafo, e se nel corso della visita incontriamo un nodo che è già stato visitato, vuol dire che esiste un ciclo: interrompiamo la ricerca e restituiamo `True`