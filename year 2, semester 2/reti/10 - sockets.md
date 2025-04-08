---
related to: "[[07 - livello trasporto]]"
created: 2025-03-02T17:41
updated: 2025-04-08T12:18
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
inoltre il [[07 - livello trasporto#numeri di porta|numero di porta]] è una delle informazioni contenute negli header di livello di trasporto per capire a quale applicazione bisogna riportare il messaggio

>[!example] esempio
per inviare un messaggio HTTP al server `gaia.cs.umass.edu`, sono necessari:
>- indirizzo IP: `128.119.245.12`
>- numero di porta: `80` (well-known) 

dato che l’interazione tra client e server è bidirezionale, è necessaria una coppia di indirizzi socket: **locale**(mittente) e **remoto**(destinatario)
- l’indirizzo locale in una direzione è l’indirizzo remoto nell’altra direzione
[[07 - livello trasporto#individuare i socket|i socket vengono individuati in questo modo]]

## utilizzo dei servizi di livello di trasporto
sappiamo già che per comunicare, i due processi devono utilizzare i servizi offerti dal livello trasporto: in particolare è importante la scelta tra TCP e UDP, in base al servizio richiesto dall’applicazione:
- **perdita di dati**: alcune applicazioni possono tollerare qualche perdita di dati, mentre altre richiedono un trasferimento dati affidabile al 100%
- **temporizzazione**: alcune applicazioni, per essere “realistiche”, richiedono piccoli ritardi, mentre altre non hanno particolari requisiti di temporizzazione
- **throughput**: alcune applicazioni, per essere efficaci, richiedono un’ampiezza di banda minima, altre utilizzano l’ampiezza di banda che si rende disponbile
- **sicurezza**: necessità di cifratura, integrità dei dati, etc.
## programmazione con socket
impareremo a costruire un’applicazione client/server che comunica utilizzando le socket. useremo la **Socket API**, introdotta nel 1981
>[!info] socket
una socket è un’interfaccia di un host locale, creata dalle applicazioni, controllata dal SO, in cui il processo di un’applicazione può inviare e ricevere messaggi da/al processo di un’altra applicazione
### programmazione socket con TCP
>[!info] rappresentazione
![[Pasted image 20250407174137.png]]

1. il client deve contattare il server (il processo server deve essere sempre in corso di esecuzione, e deve aver creato un socket che dà il benvenuto al contatto con il client) creando un socket TCP, specificando indirizzo IP e numero di porta del processo server. 
2. quando il client crea il socket, il client TCP stabilisce una connessione con il server TCP
3. quando viene contattato dal client, il server TCP crea un nuovo socket per il processo server **per comunicare con il client** (ciò consente al server di comunicare con più client)
	- vengono usati i numeri di porta origine per distinguere i client

>[!info] interazione client/server TCP
![[Pasted image 20250407174604.png]]

>[!info] terminologia
**stream** (flusso): sequenza di caratteri che fluisce da/verso un processo
**input stream** (flusso d’ingresso): è collegato a un’origine di input per il processo, ad esempio la tastiera o la socket
**output stream** (flusso di uscita): è collegato a un’uscita per il processo, ad esempio il monitor o la socket
![[Pasted image 20250408121807.png]]
# 
socket per TCP : ip e porta mittente + ip e porta destinatario 
socket per UDP :  numero di porta  + ip del destinatario (ip e porta del mittente sono comunque nel pacchetto affinchè il server possa rispondere ! quindi è solo una distinzione logica, le informazioni ci sono comunque )
interfaccia socket: un interfaccia di operazionie