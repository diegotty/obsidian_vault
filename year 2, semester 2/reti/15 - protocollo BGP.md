---
related to: "[[11 - livello di rete]]"
created: 2025-03-02T17:41
updated: 2025-05-08T11:03
completed: false
---
## internet routing
abbiamo fin qui visto la rete come una collezione di router interconnessi: ciascun router era indistinguibile dagli altri (avevamo quindi una visione omogena della rete)
ma nella pratica non è così semplice:
- **scala**: parliamo di 200 milioni di destinazioni. archiviare le informazioni d’istradamento su ciascun host richiederebbe un’enorme quantità di memoria ! e inoltre il traffico generato dagli aggiornamenti LS non lascerebbe banda per i dati, e l’algoritmo con DV non convergerebbe mai 
- **autonomia amminstrativa**: da un punto di vista ideale, nell’Internet ciascuno (ISP) dovrebbe essere in grado di amministrare la propria rete nel modo desiderato, pur mantenendo la possibilità di connetterla alle reti esterne
	- in particolare, ogni ISP è un’autorità amministrativa autonoma: usa le sottoreti che vuole e impone politiche specifiche sul traffico
## instradamento gerarchico
per accomodare questi problemi, si fa uso dell'**instradamento gerarchico**:
### sistemi autonomi
 ogni ISP è un **autonomous system** (**AS**), che può eseguire un protocollo di routing che soddisfa le sue esigenze
- i router all’interno dell’AS eseguono lo stesso protocollo di routing, chiamato **intra-AS** o **intradominio**, o **interior gateway protocol** (**IGP**)
- i router appartenenti a AS diversi possono eseguire IGP diversi !
 dobbiamo però avere un solo protocollo **inter-AS** (o **interdominio** o **exterior gateway protocol** (**EGP**)), che gestisce il routing tra i vari AS
- il protocollo **inter-AS** viene eseguito sui **router gateway**, che sono router (dentro AS) che hanno il compito di connettere gli AS tra loro, e quindi di **inoltrare pacchetti a destinazioni esterne**
	- eseguono quindi sia IGP che EGP !
gli AS possono essere di diverse dimensioni, e ad ogni AS viene assegnato dall’ICANN un numero identificativo **univoco** di 16 bit: l’**autonomous number** (**ASN**)

gli AS sono classificati in base al modo in cui sono connessi ad altri AS:
- **AS stub**: ha un solo collegamento verso un altro AS, e il traffico è generano  o destinato allo stub, ma **non transita attraverso di esso**
- **AS multihomed**: ha più di una connessione con altri AS, ma non consente transito di traffico (es: azienda che usa serivizi di più network provider, ma non fornisce connettività agli altri AS)
- **AS di transito**: è collegato a più AS, e consente il traffico (es: network provider e dorsali)

>[!info] sistemi autonomi interconnessi
![[Pasted image 20250508101650.png]]
per il **routing intra-AS**, vengono usati i protocolli che abbiamo studiato precedentemente; **RIP** o **OSPF**
per il **routing inter-AS**, viene usato il protocollo **BGP** (**border gateway protocol**)

>[!example] instradamento inter-AS
![[Pasted image 20250508101936.png]]
senza BGP, ogni router all’interno degli AS sa come raggiungere tutte le reti che si trovano nel suo AS, ma non sa come raggiungere una rete che si trova in un altro AS
## BGP
il **BGP** (**border gateway protocol**) viene usato per determinare le coppie origine-destinazione che interessano più sistemi autonomi
- è di proprietà di CISCO, e rappresenta lo standard 
BGP è un protcollo **path vector** (struttura che memorizza, oltre alla distanza come i DV, i percorsi per arrivare agli altri nodi), e mette a disposizione di ciascun AS un modo per:
- ottenere informazioni sulla raggiungibilità di sottoreti da parte di AS confinanti
- propagare le informazioni di raggiungibilità a tutti i router interni di un AS
- determinare percorsi **buoni\*** verso le sottoreti sulla base delle informazioni di raggiungibilità e delle politiche dell’AS
	- **buono\***: un percorso è buono relativamente a diversi possibili motivi, che possono essere diversi dal percorso di distanza minima: sotto il punto di vista politico, economico (un percorso più lungo può essere meno costoso), etc..
### path-vector routing
il path vector routing è l’unico (relativamente a LS e DV) che permette di tenere conto di eventuali **politiche** (““preferenze””) del mittente (un mittente può non volere che i suoi pacchetti passino attraverso determinati router)

nella pratica, è simile a DV, ma vengono inviati i percorsi invece che solo le destinazioni (con la relativa distanza)
- ogni nodo, quando riceve un path vector da un vicino, aggiorna il suo path vector **applicando la sua politica** invece del costo minimo
>[!example] esempio
![[Pasted image 20250508103154.png]]
viene applicata `migliore()`, che rispetta la politica dell’AS
![[Pasted image 20250508103258.png]]


>[!info] algoritmo path-vector
>```pseudo-code
>path_vector_routing():
>	##inizializzazione
>	path = [] * N
>	nodi = [A, B, C, D, E, F, G]
>	me_stesso = C
>	for y in nodi: # N compreso
>		if y == me_stesso
>			path[y] = me_stesso
>		else if (y è un mio vicino)
>			path[y] = me_stesso + y
>		else 
>		path[y] = vuoto
>	spedisci path a tutti i vicini
>
>	##aggiornamento
>	while(true):
>		wait for a path_w da un vicino w
>		for y in nodi:
>			if path_w comprende me_stesso:
>				ignora il percorso
>			else
>				path[y] = migliore(path[y], me_stesso + path_w[y])
>		if c'è un cambiamento nel vettore
>			spedisci path a tutti i vicini
>```

### eBGP e iBGP
come abbiamo visto, per permetter ad ogni router di instradare correttamente i pacchetti, qualsiasi sia la destinazione, è necessario istallare su tutti i **gateway router** dell’AS, una variante del BGP chiamata **eBGP**
- invece **tutti i router** (compresi quelli di confine) dovranno usare l’**iBGP** (che è diverso dal protocollo intra-dominio !!!!)
>[!info] eBGP e iBGP
![[Pasted image 20250508104842.png]]

BGP permette a coppie di router di scambiarsi informazioni di instradamento su connessioni TCP, usando la porta 179
- i router ai capi di una connessione TCP sono chiamati **peer BGP**, e la connessione TCP con tutti i messaggi BCP inviati viene chiamata **sessione BGP**
>[!info] eBGP
![[Pasted image 20250508105128.png]]
usando solo l’eBGP, le informazioni di raggiungibilità non sono complete:
>- i router di confine sanno instradare pacchetti solo ad AS vicini (con cui hanno comunicato, ma non sanno come instradare i pacchetti di cui sanno i percorsi altri gateway router nello stesso AS)
>- nessuno dei router **non** di confine sa come instradare un pacchetto destinato alle reti che si trovano in altri AS

>[!info] iBGP
![[Pasted image 20250508105439.png]]
iBGP crea una sessione tra ogni possibile coppia di router all’interno di un AS: non tutti i nodi hanno messaggi da inviare (i router non gateway), ma tutti ricevono
>- il processo di aggiornamento non termina dopo il primo scambio di messaggi ! ( i router aggiornano le proprie tabelle e le ri-mandano ai vicini), ma finisce quando non ci sono più aggiornamenti

## tabelle di routing
le informazioni ottenute da eBGP e iBGP vengono combinate per creare le tabelle dei percorsi
>[!example]- tabelle di percorso
![[Pasted image 20250508110127.png]]
![[Pasted image 20250508110247.png]]

le tabelle di percorso ottenute da BGP non vengono usate di per sè per l’instadamento dei pacchetti, bensì inserite nelle tabelle di routing intra-dominio, generate da RIP o OSPF


o
prima eBGP e poi iBGP (non basta eBGP, che fa arrivare le informazioni solo ad alcuni router (i gateway))

