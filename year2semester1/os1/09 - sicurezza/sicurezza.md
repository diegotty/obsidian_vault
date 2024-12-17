---
created: 2024-12-17
related to: 
updated: 2024-12-17T17:46
---
dal manuale sulla sicurezza informatica del NIST (national institute of standards and technology:
la **sicurezza** è la protezione offerta da un sistema informatico automatico al fine di conservare integrità, disponibilità e confidenzialità dalle risorse del sistema stesso
>[!info] la triade
>![[Pasted image 20241217171022.png]]
integrità, disponibilità e confidenzialità
esistono 2 obiettivi che vengono aggiunti al nucleo della sicurezza informatica: autenticità e tracciabilità (accountability)

**integrità**: riferita tipicamente ai dati, che non devono essere modificati senza le dovute autorizzazioni
**confidenzialità**: riferita tipicamente ai dati, che non devono essere letti senza le dovute autorizzazioni
**disponiblità**: riferita tipicamente ai servizi, che devono essere disponibili senza interruzioni
**autenticità**: riferita tipicamente agli utenti, che devono essere chi dichiarano di essere (per estensione, vale anche per dati e messaggi)
## minacce
l’RFC 2828 descrive quattro conseguenze delle minacce informatiche:
- **unauthorized disclosure**(accesso non autorizzato): un’entità ottiene l’accesso a dati per quali non ha l’autorizzazione (minacciando la confidenzialità)
	- alcuni degli attacchi usati per ottenere l’accesso non autorizzato sono:
		- esposizione 
		- intercettazione 
		- inferenza(non riesco ad accedere ai dati che mi servono, ma riesco a ricavarli da altre informazioni) 
		- intrusione (prepotenza)
- **deception**(imbroglio (lmao)): un’entità autorizzata riceve dei dati falsi e pensa che siano veri (minacciando l’integrità del sistema o dei dati)
	- alcuni degli attacchi usati to deceive sono:
		- mascheramento(un attaccante riesce ad entrare in possesso delle redenziali di un utente autorizzato: **trojan !**) 
		- falsificazione 
		- ripudio (un utente nega di aver ricevuto o inviato dei dati )
- **disruption**(interruzione): impedimento del corretto funzionamento dei servizi (minacciando l’integrità del sistema o la disponibilità dei servizi)
	- alcuni degli attacchi usati to disrupt sono:
		- incapacitazione (rompendo un qualche componente del sistema)
		- ostruzione, per esempio riempiendo il sistema di richieste(**DoS**: denial of service)
		- corruzione
- **usurpation**(usurpazione): il sistema viene direttamente controllato da chi non ne ha l’autorizzazione (minacciando l’integrità del sistema)
	- alcuni degli atttachi usati per usurpare sono:
		- appropiazione indebita, ovvero diventare amministratore di una macchina non propria
		- uso non appropriato (virus che cancella file o fa danni)

## asset
gli asset di un sistema computerizzato sono le sue risorse, e possono essere categorizzati come:
- hardware
- software
- dati
- linee di comunicazione e reti
>[!info] rappresentazione della protezione degli asset
![[Pasted image 20241217173108.png]]

### relazione tra asset e la triade
gli attacchi nella tabella danneggiano gli asset e minacciano l'elemento della triade di cui fanno parte

|                   | disponibilità                                                  | confidenzialità                                                     | integrità                                                                                     |
| ----------------- | -------------------------------------------------------------- | ------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| **hardware**      | workstation rubate o rese inutilizzabili                       |                                                                     |                                                                                               |
| **software**      | programmi cancellati                                           | copia non autorizzata dei programmi                                 | modifica dei programmi (per non farli funzionare o per fargli fare compiti indesiderati)      |
| **dati**          | file cancellati                                                | file letti senza autorizzazione. dati inferiti da analisi statisica | modifica di file esistenti o creazione di file                                                |
| **comunicazione** | messaggi distrutti. linee di comunicazione rese inutilizzabili | lettura dei messaggi o osservazione dei pattern                     | modifica, ritardo, riordino o duplicazone dei messaggi esistenti, creazione di messaggi falsi |
# autenticazione
l’autenticazione è il primo passo da parte del SO per protegere il sistema, e la base per la maggior parte dei tipi di controllo di accesso e tracciabilità
l’autenticazione ha 2 passi:
-  identificazione
- verifica
determina quindi se un utente è abilitato ad accedere al sistema, e determina anche i privilegi