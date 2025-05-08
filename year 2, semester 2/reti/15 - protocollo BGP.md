---
related to: "[[11 - livello di rete]]"
created: 2025-03-02T17:41
updated: 2025-05-08T10:19
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
## BGP

ricordiamo che oltre i 15 hop non posso andare, devo usare altri protocolli (?) (per DV)


router gateway eseguono un protocollo aggiuntivo rispetto agli altri
## border gateway protocol (BGP)
path vector: mantiene i percorsi a=

i percorsi buoni sono “buoni” sotto diversi punti di vista: politico, economico (un percors più lungo può essere migliore xke lo pago di meno) quindi non cerco sempre il percorso minimo


prima eBGP e poi iBGP (non basta eBGP, che fa arrivare le informazioni solo ad alcuni router (i gateway))

