# sistema operativo
gestisce le risorse hardware di un sistema, per fornire un insieme di servizi agli utenti, ad esempio:
- esecuzioni di programmi
- accesso a dispositivi i/o
- accesso al SO stesso (shell)
- sviluppo di programmi
- rilevamento di 
//TODO slide 64 foto
## sistemi batch
(anni Cinquanta)
nei sistemi batch, piu’ del 96% del tempo e’ sprecato ad aspettare i dispositivi di i/o
# multiprogrammazione
un processore deve eseguire piu’ programmi contemporaneamente, e la sequenza con cui i programmi sono eseguiti dipende dalla loro priorita’ e dal fatto che siano o meno in attesa di input/output
per rendere “l’illusione” dell’esecuzione di molteplici programmi in contemporanea, viene usata la multiprogrammazione.
- al posto di aspettare che le istruzioni di i/o siano completate di procedere, il processore passa ad un altro processo.
in questo modo, la percentuale d’uso del processore(e del resto delle componenti) si alza, rendendo il ciclo di esecuzione piu’ efficace. inoltre i tempi per eseguire un insieme di processi diminuisce drasticamente inoltre i tempi per eseguire un insieme di processi diminuisce drasticamente
//TODO foto 77, 78, 79
alla fine della gestione di un’interruzione, il controllo potrebbe non tornare al programma che era in esecuzione al momento dell’interruzione

## sistemi time sharing
(anni Settanta)
sistemi a condivisione del tempo! uso della multiprogrammazine per gestire contemporaneamente piu’ jobs interattivi. in questo modo il tempo del processore e’ diviso tra piu’ utenti, e piu’ utenti contemporaneamente accedono al sistema tramite terminali

|                                   | batch                                                          | time sharing                     |
| --------------------------------- | -------------------------------------------------------------- | -------------------------------- |
| scopo principale                  | massimizzare l’uso del processore (?)                          | minimizzare il tempo di risposta |
| provenienza delle direttiva al SO | comandi del job control language, sottomessi con il job stesso | comandi dati dal terminale       |
# job vs processo
il processo riunisce in un unico conetto il job non-interattivo e quello interattivo
- incorpora anche un altro tipo di job che comincio’ a manifestarsi dagli anni Settanta: quello transazionale real-time (ad es. prenotazione biglietti aerei)
il processo e’ un’unita di attivita’ caratterizata da:
- un singolo flusso (thread) di esecuzione
- uno stato corrente
- un insieme di risorse di sistema ad esso associate
dato che viene introdotto un nuovo tipo di job, la multiprogrammaziones sui processi ha delle difficolta’:
- errori di sincronizzazione
- violazione della mutua esclusione
- programmi con esecuzione non deterministica
- deadlock