---
related to:
created: 2025-03-02T17:41
updated: 2025-10-23T18:03
completed: false
---
abbiamo iniziato studiando i *DFA* (deterministic finite-state automata), in quanto la loro quantità di memoria limitata e il processing di input bit-a-bit implica degli automi più semplici.
abbiamo studiato l’insieme di linguaggi accettati da un qualunque $M \in DFA$: i *linguaggi regolari*, definendo un insieme di operazioni su tali linguaggi: unione, interesezione, complemento, concatenazione e potenza.
- abbiamo dimostrato che i linguaggi regolari sono chiusi per tali operazioni
per riuscire a dimostrare la chiusura per la concatenazione, abbiamo introdotto gli *NFA* (non-deterministic finite-state automata).
>[!warning] a questo punto è importante ricordare che l’automa accetta solo se, a fine input, ci si trova in uno stato finale

dopo aver definito gli *NFA*, abbiamo dimostrato che $\text{L(DFA)  = L(NFA) = REG}$, e abbiamo imparato come passare da *NFA* a *DFA*.
- con gli *NFA* abbiamo poi completato i teoremi di chiusura per i linguaggi regolari.

abbiamo introdotto le *espressioni regolari* (regex), e abbiamo dimostrato che $\text{REG} = \text{L(RE)}$ ( cioe $\text{un linguaggio è regolare } \iff \exists \text{ un'espressione regolare che lo descrive}$)

