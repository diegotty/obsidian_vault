---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-07T16:42
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

>[!dimostrazione] dimostrazione
>proviamo che la regola è giusta
supponiamo per assurdo che la soluzione greedy $SOL$, trovata con questa regola, non sia ottima. le soluzioni ottime differiscono dunque da $SOL$ ! 