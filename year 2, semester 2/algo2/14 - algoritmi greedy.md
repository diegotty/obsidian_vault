---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-07T16:58
completed: false
---
# algoritmi greedy
per illustrre la progettazione e l’analisi di un algoritmo **greedy**, consideriamo un problema piuttosto semplice chiamato **selezione di attività**:
>[!info] selezione di attività
>abbiamo una lista di $n$ attività da eseguire:
>- ciascuna attività è caratterizzata da una copia: (tempo_inizio, tempo_fine)
>- due attività sono **compatibili** se non si sovrappongono
>
>vogliamo trovare un sottoinsieme di **massima cardinalità** di attività compatibili 

>[!example] esempio
![[Pasted image 20250407163452.png]]
in generale possono esistere diverse soluzione ottime, ma in questo caso è facile convincersi che l’unico sottoinsieme massimale è $\{b,e,h\}$

volendo utilizzare il paradigma **greedy**, dovremmo trovare una regola, semplice da calcolare, che ci permetta di effettuare ogni volta la scelta giusta

per questo problema ci sono diverse potenziali regole di scelta:
- prendi l’attività compatibile che inizia prima
- prendi l’attività compatibile che dura di meno
- prendi l’attività compatibile che ha meno conflitti con le rimanenti
(esistono controesempi per tutte queste)
**la regola giusta sta nel prendere sempre l’attività compatibile che finisce prima** !!
>[!example]- esempio con regola giusta
![[Pasted image 20250407164028.png]]

>[!dimostrazione] dimostrazione della correttezza della regola
supponiamo per assurdo che la soluzione greedy $SOL$, trovata con questa regola, non sia ottima. le soluzioni ottime differiscono dunque da $SOL$ ! 
>nel caso dci fossero più di una soluzione ottima, prendiamo quella che differisce nel minor numero di attività da $SOL$: sia $SOL*$. dimostreremo che esiste un’altra soluzione ottima $SOL'$ che differisce ancora meno da $SOL$, il che è assurdo (stesso tipo di dimostrazione dello spanning tree)
>
>siano $A_{1}, A_{2}, \dots$, le attività nell’ordine in cui sono state selezionate dall’algoritmo. sia $A_{i}$ la prima attività scelta dal greedy e non dall’ottimo (che deve esistere, in quanto tutte le attività non prese dal greedy sono incompatibili con le attività scelte dal greedy, e se la soluzione avesse preso tutte le attività scelte dal greedy, non potrebbe averne prese di più: quindi il greedy deve avere attività diverse).
>nell’ottimo deve esserci un’altra attività $A'$ che va in conflitto con $A_{i}$, altrimenti $SOL*$ non sarebbe ottima in quanto potrei aggiungerci $A_{i}$.
>a questo punto, posso sostituire in $SOL*$ l’attività $A'$ con l’attività $A_{i}$, senza creare conflitti (in quanto per la regola di scelta greedy, $A_i$ termina prima di $A'$(e l’attività prima è uguale per entrambe le soluzioni)).
>ottengo in questo modo una soluzione ottima $SOL'$ con la stessa cardinalità di $SOL*$, che differisce da $SOL$ di un’attività in meno rispetto a $SOL*$.
>![[Pasted image 20250407165234.png]]

>[!info] implementazione
pseudocodice:
>```
>def selezione_a(lista):
>	SOL = []
>	while lista:
>		estrai da lista l'attività (a,b) che termina prima
>		if (a,b) non va in conflitto con le attività in SOL: 
>			SOL.append((a,b))
>	return SOL
>```
**IDEE**
>- per l’implementazione, conviene fare un preprocessing ordinando gli eventi per tempo di fine crescente, pagando $O(n\log n)$ per l’ordinamento ma poi avendo l’estrazione delle attività in $\Theta(1)$
>- per eseguire efficien

```python
```

