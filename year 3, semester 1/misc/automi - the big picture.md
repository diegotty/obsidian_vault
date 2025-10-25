---
related to:
created: 2025-03-02T17:41
updated: 2025-10-25T15:15
completed: false
---
abbiamo iniziato studiando i *DFA* (deterministic finite-state automata), in quanto la loro quantità di memoria limitata e il processing di input bit-a-bit implica degli automi più semplici.
abbiamo studiato l’insieme di linguaggi accettati da un qualunque $M \in DFA$: i *linguaggi regolari*, definendo un insieme di operazioni su tali linguaggi: unione, interesezione, complemento, concatenazione e potenza.
- abbiamo dimostrato che i linguaggi regolari sono chiusi per tali operazioni
per riuscire a dimostrare la chiusura per la concatenazione, abbiamo introdotto gli *NFA* (non-deterministic finite-state automata).

dopo aver definito gli *NFA*, abbiamo dimostrato che $\text{L(DFA)  = L(NFA) = REG}$, quindi che possono rappresentare gli stessi linguaggi, e abbiamo imparato come passare da *NFA* a *DFA*.
- con gli *NFA* abbiamo poi completato i teoremi di chiusura per i linguaggi regolari.

abbiamo introdotto le *espressioni regolari* (regex), e abbiamo dimostrato che $\text{REG} = \text{L(RE)}$ ( cioe $\text{un linguaggio è regolare } \iff \exists \text{ un'espressione regolare che lo descrive}$)
per completare la dimostrazione, abbiamo usato i *GNFA* (NFA generalizzati, che sugli archi hanno espressioni regolari).
- in particolare, abbiamo usato la loro forma canonica, creato una funzione `convert()` ricorsiva che elimina i nodi intermedi, uno alla volta, fino a rimanere con 2 stati e 1 arco: l’espressione regolare del linguaggio
	- abbiamo anche dimostrato che `convert()` crea un automa equivalente a quello di partenza

abbiamo studiato il *pumping lemma*, che permette di dimostare se un linguaggio è regolare o meno.

>[!warning] a questo punto è importante ricordare che l’automa accetta solo se, a fine input, ci si trova in uno stato finale