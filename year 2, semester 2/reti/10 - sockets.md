---
related to: "[[07 - livello trasporto]]"
created: 2025-03-02T17:41
updated: 2025-04-07T17:27
completed: false
---
nel paradigma client/server, la comunicazione a livello applicazione avviene tra due programmi applicativi in esecuzione chiamati **processi**; un client e un server:
- un **client** è un programma in esecuzione che inizia la comunicazione inviando una richiesta
- un **server** è un altro programma applicativo che attende le richieste dai client
## API
anche se un linguaggio di programmazione prevede vari insiemi di istruzioni (es: istruzioni matematiche, manipolazione di stringhe, per la gestione dell’input/output), se si vuole sviluppare un programma capace di comunicare con un altro programma, è necessario un nuovo insieme di istruzioni, per chiedere ai primi 4 livelli dello stack TCP/IP di aprire la connessione, inviare/ricevere dati e chiudere la connessione
un insieme di istruzioni di questo tipo viene chiamato **API** (application programming interface)
>[!info] interfaccia socket nello stack TCP/IP
![[Pasted image 20250406180128.png]]
# socket
il socket appare come un terminale o un file, ma non è un’entità fisica, bensi un’astrazione: una struttura dati creata e utilizzata dal programma applicativo
>[!info] 
![[Pasted image 20250406180327.png]]
>comunicare tra un processo client e un processo server signfica comunicare tra due socket create nei due lati di comunicazione

>[!info] socket address
![[Pasted image 20250406181407.png]]
## indirizzamento dei processi
affinchè un processo su un host invii un messaggio a un processo su un altro host, il mittente deve identificare il processo destinatario. un host ha un indirizzo IP univoco a 32 bit, **ma non è sufficiente** per identificare il processo di un host, in quanto in un host possono essere in esecuzione molti processi.
>[!warning] l’identificare comprende quindi sia l’indirizzo ip, che i **numeri di porta** associati al processo in esecuzione su un host
inoltre il numero di porta è una delle informazioni contenute negli header di livello di trasporto per capire a quale applicazione bisogna riportare il messaggio

>[!example] esempio
per inviare un messaggio HTTP al server `gaia.cs.umass.edu`, sono necessari:
>- indirizzo IP: `128.119.245.12`
>- numero di porta: `80` (well-known) 
# 
socket per TCP : ip e porta mittente + ip e porta destinatario 
socket per UDP :  numero di porta  + ip del destinatario (ip e porta del mittente sono comunque nel pacchetto affinchè il server possa rispondere ! quindi è solo una distinzione logica, le informazioni ci sono comunque )
interfaccia socket: un interfaccia di operazionie