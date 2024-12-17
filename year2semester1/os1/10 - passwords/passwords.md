---
created: 2024-12-17
related to: 
updated: 2024-12-17T21:26
---
una password è un dato segreto, tipicamente utilizzato per identificare un utente, e garantire l’accesso sicuro
# password in Linux
per gestire utenti e le relative password, Linux usa 2 file:
- `\etc\passwd`
- `\etc\shadow`
entrambi sono normali file di testo con una sintassi simile, ma hanno funzionalità e permessi diversi
- per ogni riga (utente) in `\etc\passwd`, esiste un a riga in `\etc\shadow` che indica la sua password
- originariamente, esisteva solo il file *passwd*, che includeva la password dell’utente in plaintext….. la struttura è stata cambiata x ovvi motivi
## `\etc\passwd`
è un file in plaintex, che contiene l’intera lista di utenti presenti nel sistema
- include non solo gli utenti “normali”, ma anche utenti standard di sistemi e utenti speciali (ad esempio l’utente *nobody*, che solitamente è usato per dare il minimo set di permessi possibili ad un processo)
di default, `\etc\passwd` ha i seguenti permessi:
```
-rw-r--r-- 1 root root 2659 Dec 22 12:21 /etc/passwd
```
# attacchi a password
# sviluppi futuri