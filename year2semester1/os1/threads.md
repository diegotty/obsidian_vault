---
created: 2024-10-15
related to: "[[gestione dei processi]]"
updated: 2024-10-15, 08:46
---
# thread
fino a quanto studiato, ogni processo compete con tutti gli altri per l’esecuzione: grazie a una parallelizzazione che può essere:
- finta: c’è solo un processore, i processi vengono swappati troppo in fretta perchè l’utente se ne accorga
- vera: ci sono più processori (o un processore con più core)
per alcune applicazioni, però, è importante che esse siano a loro volta organizzate in parallelo
>[!example]
per esempio, un’applicazione grafica potrebbe essere suddivisa in 3 computazioni: una che ascolta input, una che ridisegna la finestra, e un’altra che fa i calcoli necessari

alcune applicazioni vengono suddivise a loro volta in più esecuzioni: i thread
i diversi thread in un processo condividono quasi tutte le risorse(tra cui memoria, files, dispositivi di I/O), tranne:
- stack delle chiamate e quindi variabili locali
- il processore

>[!figure] ![[Pasted image 20241015085415.png]]
>modi diversi di gestire i thread 

>[!figure] ![[Pasted image 20241014195251.png]]
user addres space: la parte dell’immagine condivisa tra i thread
thread control block: gestisce solo la parte di scheduling
