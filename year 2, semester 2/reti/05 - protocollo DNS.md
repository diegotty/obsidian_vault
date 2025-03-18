---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-19T00:32
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

>[!info]- DNS centralizzato
>inoltre, DNS non può essere centralizzato, in quanto se si memorizzasse tutto il database su un singolo server:
>- ci sarebbe un singolo punto di fallimento
>- il volume di traffico sarebbe troppo elevato 
>- ci sarebbe sempre, per qualcuno, troppa distanza dal database centralizzato
>- la manutenzione sarebbe ingestibile: il server dovrebbe essere aggiornato di continuo per includere nuovi nomi di host
>quindi un database centralizzato su un singolo server DNS non è scalabile :(

la **gerarchia DNS** è organizzata in questo modo: le informazioni si organizzano in base al dominio, e ci sono 3 classi di server DNS organizzati in una gerarchia:
- **root**
- **TLD**(top-level domain)
- authoritative
- ci sono poi i server DNS **locali** con cui interagiscono direttamente le applicazioni
>[!figure] gerarchia server DNS
![[Pasted image 20250318213556.png]]
### server DNS root
in Internet ci sono 13 server DNS root, e ognuno di questi server è replicato per motivi di **sicurezza** e **affidabilità**: in totale diventano 247 root server (19 copie per ogni server)
>[!figure] disposizione DNS root server
![[Pasted image 20250318213903.png]]
### TLD
i server TLD si occupano dei domini `com`, `org`, `net`, `edu`, etc …, e di tutti i domini locali di alto livello, quali `it`, `uk`, `fr`, `ca`,  e`jp`.
- in particolare la compagnia Verisign Global Registry Services gestisce i server TLD per il dominio `com`
- in particolare il registro.it che ha sede a Pisa al CNR gestice il dominio `it`
#### etichette dei domini generici
| etichetta | descrizione                            |
| --------- | -------------------------------------- |
| aero      | companie aree e aziende aerospaziali   |
| biz       | aziende (simile a com)                 |
| com       | organizzazioni commerciali             |
| coop      | associazioni di cooperazione           |
| edu       | istituzioni educative                  |
| gov       | istituzioni governative                |
| info      | fornitori di servizi informatici       |
| int       | organizzazioni internazionali          |
| mil       | organizzazioni militari                |
| museum    | musei                                  |
| name      | nomi di persone                        |
| net       | organizzazioni che si occupano di reti |
| org       | organizzazioni senza scopo di lucro    |
| pro       | organizzazioni professionali           |
### authoritative server
gli authoritative server (o server di comptetenza) possono essere mantenuti:
- dall’organizzazione che, essendo dotata di hosts Internet pubblicamente accessibili, deve fornire i record DNS di pubblico dominio che mappano i nomi di tali host in indirizzi IP
- da un ISP per l’organizzazione
in genere sono due server, primario e secondario !
>[!example] esempio di gerarchia server DNS
![[Pasted image 20250318214607.png]]
### server DNS locale
quando un host effettua una richiesta DNS, la query viene inviata al suo server DNS locale, che opera da proxy e inoltra la query in una gerarchia di server DNS !
- il server DNS locale non appartiene strettamente alla gerarchia dei server, e ciascun ISP ha un server DNS locale, detto anche “default name server”
## query DNS
>[!info] query iterativa
![[Pasted image 20250318215327.png]]
ogni volta che il DNS locale fa una query, esso stesso riceve la risposta da uno dei livelli della gerarchia DNS e si occupa di inviare un’altra richiesta al livello della gerarchia DNS inferiore !

>[!info] query ricorsiva
![[Pasted image 20250318215502.png]]
in questo tipo di query, l’host richiedente (e il server DNS locale per lui) affida il compito di tradurre il nome al server DNS contattato
## DNS caching
DNS sfrutta il caching per migliorare le prestazioni di ritardo e per ridurre il numero di messaggi DNS che “rimbalzano” in Internet: una volta che un server DNS impara la mappatura, la mette nella **cache** (duh), secondo i criteri:
- le informazioni nella cache vengono invalidate, e quindi spariscono, dopo un certo periodo di tempo
- tipicamente un server DNS locale memorizza nella cache gli indirizzi IP dei server TLD, ma anche quellli di competenza
	- quindi i server DNS radice non vengono vistati spesso !
## DNS record
il **mapping** è contenuto nei database sotto forma di **resource record**(**RR**): ogni RR mantiene un mapping(es: tra hostname e indirizzo IP, oppure tra alias e nome canonico, etc)
i record vengono quindi spediti tra server e all’host richiedente all’interno di messaggi DNS
- un messaggio può contenere più RR !



i resource record vengono iniviati all’interno dei messaggi DNS