---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-15T19:06
completed: false
---
# filesytem
i file regolari contengono sequenze di bit dell’area di memoria sulla quale c’è il filesystem 
- possono essere ASCII o binari
esiston anche **file non regolari, o speciali**, che modellano unità di I/O
- possono essere a blocchi/caratteri
>[!tip] ~ equivale a /home/userX
>ed è la home directory del current user

>[!info] fun facts .. 
>- quando si usa `cd`, `/./` fa rimanere allo stesso posto. (huh.)
>- path assoluto è valido qualunque sia la `cwd`, mentre il path relativo può non essere valido quando si cambia `cwd`
## mounting
li filesystem root (`/`) contiene elementi eterogenei: 
- disco interno
- filesystem su disco esterno
- filesystem di rete
- filesytem virtuali (usati dal kernel per gestire risorse)
- filesystem in memoria principale..
una qualsiasi directory $D$ dell’albero gerarchico può diventare il punto di mount per un altro (nuovo) filesystem $F$ se e solo se la directory root di $F$ può essere accessibilie di $D$
- se $D$ è vuota, dopo il mount conterrà $F$
- se $D$ non è vuota, dopo il mout conterrà $F$, ma ciò non significa che i dati che vi erano dentro sono persi: saranno di nuovo accessibili dopo l’unmount di $F$
>[!info] partizioni e mounting
un singolo disco può essere diviso in due o più partizioni ! e ciò ha molto senso. presumiamo che una partizione $A$ contenta il SO e la partizione $B$ contenga i dati degli utenti.
>- se devo reinstallare il SO in $A$, non tocco i dati e mi basta rimontare $B$, e se voglio installare diverse distro di linux in altre partizioni (es: $C$), posso allo stesso modo avere i dati degli utenti senza copiarli, ma semplicemente montando $B$
### tipi di filesystem
come sappiamo da [[filesystem in UNIX, Windows, Linux|os1]], esistono diversi tipi di filesystem. dal punto di vista del programmatore, il tipo di filesystem definisce la codifica dei dati, mentre dal punto di vista dell’utente cambiano la dimensione massima di una partizione, di un file, di un nome di file, e se usa il journaling oppure no
>[!example] filesystem non linux
>esistono ovviamente filesystem “non linux”: NTFS, MSDOS, FAT32, FAT64
>- FAT ed NTFS possono essere montati su di un filesystem linux (lo sappiamo già !)

`mount` è il comando per montare un filesystem, e visualizzare i filesystem montati
## file `passwd` e `group`
si trovano entrambi in `/etc/`, e il loro uso rappresenta una delle filosofie di linux: usare file di testo con una struttura definita e conosciuta dai programmi che devono interagire con quei file (in questo caso, `adduser` conosce la struttura di questi due file)
la struttura è stata spiegata in [[sicurezza]]
## i file
come visto in [[filesystem in UNIX, Windows, Linux#inode]], ogni file nel filesystem è rappresentato da una struttura dati **inode**, e ogni file è unicamente identificato da un **inode number**. 
- la cancellazione di un file libera l’inode number che verrà riutilizzato quando necessario per un nuovo file
>[!info] come viene seguito un path
![[Pasted image 20250315190353.png]]

per visualizzare gli inode, basta usare il flag `-i` con il comando `ls`
>[!info] ls -l
![[Pasted image 20250315190629.png]]
la dimensione