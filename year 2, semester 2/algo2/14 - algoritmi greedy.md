---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-24T07:59
completed: false
---
# algoritmi greedy
per illustrare la progettazione e l’analisi di un algoritmo **greedy**, consideriamo un problema piuttosto semplice chiamato **selezione di attività**:
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
>- per l’implementazione, conviene fare un preprocessing ordinando gli eventi per tempo di fine crescente, pagando $\Theta(n\log n)$ per l’ordinamento ma poi avendo l’estrazione delle attività in $\Theta(1)$
>- per eseguire efficientemente il test richiesto dall’if, conviene mantere una variabile con il tempo di fine dell’ultima attività inserita in SOL. in questo modo il test richiederà $\Theta(1))$
>```python
>def selezione_a(lista):
>	lista.sort(key = lambda x: x[1])
>	libero = 0
>	sol = []
>	for inizio, fine in lista:
>		if libero < inizio:
>			sol.append((inizio, fine))
>			libero = fine
>	return sol
>```
>il costo totale dell’algoritmo è $\Theta(n\log n)$

affrontiamo ora un nuovo prolema !
>[!info] assegnazione di attività
abbiamo una lista di attività, ciascuna caratterizzata da un tempo di inizio ed un tempo di fine. le attività **vanno tutte eseguite** e vogliamo assegnarle al minor numero di aule, tenendo conto che in una stessa aula non possono eseguirsi più attività in parallelo

>[!example] esempio
![[Pasted image 20250424074713.png]]

>[!info]- possibile algoritmo greedy (non corretto)
un possibile algoritmo **greedy** si basa sull’idea di occupare aule finchè ci sono attività da assegnare, e ad ogni aula, un volta inaugurata, assegnare il maggior numero di attività non ancora assegnate che è in grado di contere
>```python
>def assegnazione_a(lista):
>	i, sol = 0, []
>	while lista:
>		i += 1
>		lista1 = selezione_a(lista)
>		sol.append(lista1)
>		elimina da lista le attività in lista1
>	return sol
>```
>ma si può notare che l’algoritmo non è corretto: 
![[Pasted image 20250424075329.png]]

arriviamo allora alla soluzione corretta:
```
def assegnazione_a(lista):
	inizializza la soluzione con una aula senza attività
	while lista != []:
		estrai da lista l'attività (a,b) che inizia prima
		if c'è nella soluzione un'aula in cui è possibile eseguirla:
			assegna (a,b) a quell'aula
		else:
			inserisci nella soluzione una nuova aula ed assegnagli (a,b)
	return la soluzione con le aule e le attività assegnate
```

>[!dimostrazione] correttezza dell’algoritmo
sia $k$ il numero di aule utilizzate dalla soluzione: faremo vedere che ci sono nella lista $k$ attività incompatibili a copiia, e questo ovviamente implica che $k$ aule sono necessarie
>
>sia $(a,b)$ l’attività che ha portato all’introduzione nella soluzione della $k-esima$ aula. in quel momento nelle altre $k-1$ aule era impossibile eseguire l’attività $(a,b)$, ma per il criterio di scelta greedy posso anche dire che nell’istante di tempo $a$ tutte le aule erano occupate (le attività a loro assegnate iniziava)