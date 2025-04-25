---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-25T15:43
completed: false
---
# programmazione di sistema
come abbiamo visto nel primo modulo, il **kernel** è il componente del sistema operativo che gestisce le risorse disponibili ed offre l’accesso e l’utilizzo della risorse da parte dei processi
le principali risorse gestite dal kernel sono CPU, RAM e dispositivi di I/O
le **system call** offrono **un limitato numero** (che varia con la versione del sistema) di punti di accesso al kernel: permettono ad un processo di accedere ai servizi offerti dal kernel, e di conseguenza permettono al programmatore di creare programmi che interagiscono direttamente con il kernel !
>[!info]
![[Pasted image 20250425153955.png]]
## system call
le systemcall sono definite in linguaggio C, indipendentemente dalla tecnica usata dallo specifico sistema per invocare la system call, e **per ogni** system call esiste una funzione C avente lo stesso nome
- quando viene chiamata una syscall, il processo utente invoca questa funzione usando la sequenza standard C, che a sua volta invoca il corrispondente servizio del kernel utilizzando la tecnica necessaria (…….), ad esempio  mettendo i parametri della funzione C in un registro e poi generando un interrupt per il kernel, usando istruzioni macchina