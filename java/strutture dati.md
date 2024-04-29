caratteristiche che distinguono le strutture dati:
- è necessario mantenere un ordine ? 
- gli oggetti nella struttura possono ripetersi ?
- è necessario possedere una “chiave” per accedere a uno specifico oggetto ?
# Collection
- le collezioni in java sono rese disponibili mediante il Java Collection Framework
- sono strutture dati già pronte all’uso, con interfacce e algoritmi per manipolarle
- contengono e strutturano riferimenti ad altri oggetti (tipicamente tutti dello stesso tipo)

alcune inte

| Interfaccia | Descrizione                                             |
| ----------- | ------------------------------------------------------- |
| Collection  | L’interfaccia alla radice della gerarchia di collezioni |
| Set         | una collezione senza duplicati                          |
| List        | una collezione ordinata che può contenere duplicati     |
| Map         | associa coppie (chiave, valore) senza chiavi duplicate  |
| Queue       | Una collezione FIFO, che modella una coda               |