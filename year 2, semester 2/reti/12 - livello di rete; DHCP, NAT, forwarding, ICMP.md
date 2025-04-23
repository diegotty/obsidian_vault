---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-23T09:02
completed: false
---
# indirizzamento IPv4
**ogni interfaccia** di host e router di Internet ha un indirizzo IP globalmente univoco a 32 bit !
- per **interfaccia** si intende il confine tra host e collegamento fisico (quindi tipo una porta **fisica** ?)
	- i router devono necessariamente essere connessi ad almeno due collegamenti, mentre un host, in genere, ha un’interfaccia
>[!example]- esempio
![[Pasted image 20250423084857.png]]

gli indirizzi IPv4 sono in totale $2^{32}$ (ovvero più di 4 miliardi), e possono essere scritti in notazione binaria, decimale puntata, o esadecimale
## gerarchia nell’indirizzamento
>[!info] gerarchia nell’indirizzamento
![[Pasted image 20250423085141.png]]
ogni indirizzo IP si divide in **prefisso**, che individua la rete, e **suffisso**, che individua il collegamento al nodo
>il prefisso può avere lunghezza:
>- **fissa**: indirizzamento con classi
>- **variabile**: indirizzamento senza classi
### indirizzamento con classi
data la necessità di supportare sia reti piccole che grandi, il prefisso viene diviso in **classi**
>[!info] classi di indirizzamento
![[Pasted image 20250423085724.png]]
ci sono **3 lunghezze** di prefisso: 8, 16 e 24bit

**vantaggi**:
- in questo modo, una volta individuato l’indirizzo, si può facilmente risalire alla classe e la lunghezza del prefisso

**svantaggi**:
- è facile che gli indirizzi vengano esauriti/sprecati:
	- la classe A può essere assegnata solo a 128 orgnizzazioni nel mondo, e ognuna ha 16.777.216 nodi: la maggior parte degli indirizzi verrebbe sprecata, e poche organizzazioni potrebbero usufruire di indirizzi di classe A
	- lo stesso problema occorre per la classe B
	- per la classe C, sono disponibili solo 256 host per ogni rete (pochi)
### indirizzamento senza classi




loopback: 



dhcp usa udp (trasp non affidabile)
i messaggi dhcp vengono mandati in broadcast !

### NAT
gli indirizzi privati sono univoci per ogni LAN, ma tra LAN diverse ci possono essere dispositivi con ip uguali (non possono quindi “usicre” dalla rete con questi ip,v anno convertiti in indirizzo pubblico)

## ICMP
ICMP permette di notificare errori, e autia il protocollo IP

>[!example] ICMP 
![[Pasted image 20250410153256.png]]

ICMP viene usato da host e router per scambiarsi informazioni a livello di rete, tra cui report degli error: host, rete, porta, protocollo irraggiungibili