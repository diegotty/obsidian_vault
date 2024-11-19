---
created: 2024-11-18
related to: 
updated: 2024-11-19T07:51
---
il file system è una delle parti del SO che sono più imporanti per l’utente
proprietà desiderabili
- esistenza a lungo termine
- condivisibilità con altri processi (tramite nomi simbolici)
- strutturabilità (directory gerarchice)

i file sono gestiti da un insieme di programmi e librerie di utilità. tali programmi costituiscono il **File(Management)System**, e vengono eseguiti come processi privilegiati (**in kernel mode**)
- le librerie vengono invece invocate come syscall (quindi comunque in kernel mode)
- tali programmi e librerie forniscono un’astrazione sotto forma di operazioni tipiche(creazione, cancellazione, apertura, lettura, scrittura, chiusura) , e mantengono dei dati sui file (metadati: proprietario, data di creazione, etc…)