---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-10T15:35
completed: false
---
## indirizzamento IPv4
**ogni interfaccia** di host e router di Internet ha un indirizzo IP globalmente univoco a 32 bit !
### indirizzamento con classi
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