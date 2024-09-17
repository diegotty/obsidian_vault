Un design pattern e una soluzione concettuale che mira a risolvere uno specifico problema, spesso comune, durante la progettazione di del software.
I design pattern non sono del codice, bensi illustrano un concetto, sono una traccia la cui implementazione varia a seconda del contesto in cui viene applicato il pattern. 
Essi sono simili ad un algoritmo, in quanto mirano a fornire una soluzione ad un problema, ma, invece di delineare in modo preciso tutti i passi della soluzione, i design pattern ne forniscono una descrizione piu astratta e di alto livello.

Sfruttando i design pattern, essendo soluzioni ben “testate”, si puo essere certi di star sfruttando a pieno le capacita della programmazione orientata ad oggetti per gestire richieste 

Il pattern Decotorator permette di aggiungere funzionalita ad una classe in modo dinamico, senza modificarne il comportamento (senza modificare il suo codice). Alla base della soluzione di questo problema, viene utilizzato il principio che le classi dovrebbero essere chiuse alla modifica, ma aperte all’estensione da parte di altre classi.
Nel Decorator Pattern pero, l’estensione non viene effettuata nel modo canonico, creando una sottoclasse che estende la classe a cui aggiungere delle funzionalita, bensi creando una classe “wrapper” che e composta da un oggetto della classe a cui aggiungere.