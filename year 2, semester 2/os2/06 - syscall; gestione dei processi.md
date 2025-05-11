---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-11T21:05
completed: false
---
# gestione dei processi
## creazione
`init` è il processo 0 (con `pid=1`), padre di tutti i processi in esecuzione del sistema.
da `init` vengono creati tutti i processi, mediante la syscall `fork()`, che consisten nella duplicazione del processo, e della creazione di relazioni **padre-figlio**!

>[!example] esempio
notiamo come in questo caso, il terminale su cui è stato lanciato `pstree` è figlio di i3
![[Pasted image 20250511210433.png]]