---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-18T21:14
completed: false
---

# indirizzo IP
gli host Internt hanno i propri **hostname** (`www.google.com`, `w3.uniroma1.it`, ….), ma i nomi, anche se facili da ricoradare, forniscono poca informazione sulla collocazione degli host all’interno dell’Internet(`.it` ci dice che l’host si trova probabilmente in Italia, ma non dove)
un **indirizzo IP** ci permette di identificare un host in una rete, e anche si sapere dove si trova tale host !
- consiste di 4 byte, ed è costituito da una stringa in cui ogni punto separa dei byte espressi con un numero decimale da 0 a 255
- presenta una struttura gerarchica
# DNS
ora che abbiamo un hostname ed un indirizzo IP, dobbiamo trovare un sistema per matcharli, memorizzarli, e renderli accessibili
il **DNS**(domain name system) è un protocollo del livello applicazione che rende ciò possibile:
- **memorizazione** → database distribuito, implementato in una gerarchia di server DNS
- **accessibilità** → protocollo a livello di applicazione che consente agli host di interrogare il database distribuito per **risolvere**(tradurre) i nomi(in indirizzi)
>[!info] il DNS viene quindi usato dagli altri protocolli di livello applicazione per tradurre hostname in indirizzi IP
>utilizza il trasporto UDP e indirizza la porta 53 ! wooooo (…)

>[!example] esempio di interazione con HTTP
un bowser(ossia client HTTP) di un host utente richiede l’URL `www.someschool.edu`
>1. l’host esegue il lato client dell’applicazione DNS
>2. il browser estrae il nome dell’host, `www.someschool.edu` dall’URL e lo passa al lato client dell’applicazione DNS
>3. il cient DNS invia un query contenente l’hostname a un server DNS
>4. il client DNS riceve una risposta che include l’indirizzo IP corrispondente all’hostname
>5. ottenuto l’indirizzo IP dal DNS, il browser può dare inizio alla connessione TCP verso il server HTTP localizzato a quell’indirizzo IP

## aliasing
il DNS offre il servizio di **aliasing**: un host può avere uno o più **alias**(sinonimi)
>[!example] alias
>`relay1.west-coast.enterprise.com` potrebbe avere due sinonimi, quali `enterprise.com` e `www.enterprise.com`
>- `relay1.west-coast.enterprise.com` è un **hostname canonico**
>- `enterprise.com` e `www.enterprise.com` sono **alias**

gli alias sono più facili da ricordare obv
## distribuzione del carico
DNS viene utilizzato per distribuire il carico tra **server replicati**: i siti con molto traffico vengono replicati su più server, e ciascuno di questi gira su un sistema terminale diverso, con indirizzo IP differente
con il DNS è possibile associare un hostname canonico ad un insieme di indirizzi IP (contenuto dal DNS). in questo modo, quando viene fatta una richiesta al DNS per tale hostname, il server risponde con l’insieme di indirizzi, ma variando l’ordinamento ogni volta, effettivamente distribuendo il traffico sui server replicati
## gerarchia server DNS
>[!info] lore 
>ai tempi di ARPANET, il DNS era un file `host.txt`, mentre ora è un’applicazione che gira su ogni host, con un grande numero di server DNS distribuiti per il mondo, ed un relativo protocollo a livello applicazione che specifica la comunicazione tra server DNS e host richiedenti

inoltre, DNS non può essere centralizzato, in quanto se si memorizzasse tutto il database su un singolo server:
- ci sarebbe un singolo punto di fallimento
- il volume di traffico sarebbe troppo elevato 
- ci sarebbe sempre, per qualcuno, troppa distanza dal database centralizzato
- la manutenzione sarebbe ingestibile: il server dovrebbe essere aggiornato di continuo per includere nuovi nomi di host
quindi un database centralizzato su un singolo server DNS non è scalabile :(

i primi 2 livelli sono stati creati per rendere la ricerca veloce !


### DNS caching

i resource record vengono iniviati all’interno dei messaggi DNS