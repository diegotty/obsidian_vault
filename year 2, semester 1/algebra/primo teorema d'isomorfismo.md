---
related to: 
created: 2025-03-02T17:41
updated: 2025-07-04T12:26
completed: false
---
ALLOORAAAAAA
capiamo il primo teorema d'isomorfismo come dio comanda (e per l'ultima volta si spera)

## teorema di struttura per le applicazioni
- ho una applicazione $f: X \rightarrow Y$
- creo una relazione d'equivalenza $\tilde{ }$, in cui $x \sim x' \iff f(x) = f(x')$
- posso quozientare X per $\sim$ (ottengo tutte le classi d'equivalenza per $\sim$, cioè gli insiemi di elementi che hanno la stessa immagine )
- in questo modo, posso avere un'applicazione biettiva, $\phi: X/\sim \, \rightarrow f(X)$ in quanto tutti gli elementi della stessa classe di equivalenza hanno la stessa immagine, quindi $\phi$ è ben definita !

## primo teorema d'isomorfismo
>[!info] enunciato
dato $f$ omom, esiste $\phi$ isomorfismo tc. $\phi: G/Ker(f) \rightarrow Im(f)$
- ricalco il teorema di struttura, ma con un'aggiunta: mi rendo conto che la relazione creata per definire in modo corretto G/H (con H sottogruppo normale di G), cioè $x \sim_{1} x` \iff x(x')^-1 \in H$ e $\sim$ definita sopra
    - ciò sse utilizziamo per quozientare $H = Ker(f)$ ( $G/Ker(f)$ = tutti gli insiemi di elementi che hanno la stessa immagine (scelgo $Ker(f)$ in quanto sviluppando $f(x) = f(x')$ con le proprietà dell'omomorfismo, arrivo a $x \sim x’ \iff f(x(x')^{-1}) = 1_{G_{2}}$)
    - quindi uso effettivamente la relazione $\sim_{1}$ come definita per G/H, e dato che la relazione $\sim_{1}$ dipende da un insieme H, uso come insieme $Ker(f)$, in quanto ciò mi garantisce di ottenere classi di equivalenza con la stessa immagine per f
- di conseguenza, (del teorema di struttura) $G/\sim \, = G/H$ (del teo. d'isomorfismo)
- basta poi dimostrare che $\phi$ è un omomorfismo

>[!warning] relazione per quozientare G
>di base, per quozientare un gruppo non è necessario un isomorfismo su cui definire una relazione. basta infatti usare la relazione d’equivalenza naturale $x \sim x' \iff x(x')^{-1} \in H$
>nel primo teorema d’isomorfismo, dimostriamo che la relazione usata nel teorema di struttura per le applicazioni è lo stesso della relazione naturale tra $G$ e $H$, se e solo se $H=Ker(f)$
quindi usiamo sempre la relazione naturale per ottenere $G/Ker(f)$, ma essendo uguale alla relazione del teo. struttura ci permette di fattorizzare l’omomorfismo
