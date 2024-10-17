---
created: 2024-10-17
related to: "[[progettazione, problemi e vincoli#dipendenze funzionali]]"
updated: 2024-10-17, 07:24
---
>[!index]
>- [chiave e superchiave](#chiave%20e%20superchiave)
>- [chiave primaria](#chiave%20primaria)
>- [dipendenze funzionali banali](#dipendenze%20funzionali%20banali)
>	- [proprietà di dipendenze funzionali](#propriet%C3%A0%20di%20dipendenze%20funzionali)
>- [$F^A$](#$F%5EA$)
>	- [assioma della riflessività](#assioma%20della%20riflessivit%C3%A0)
>	- [assioma dell’aumento](#assioma%20dell%E2%80%99aumento)
>	- [assioma della transitività](#assioma%20della%20transitivit%C3%A0)

dato uno schema R e un insieme di dipendenze funzionali F, un’istanza legale di R oltre a soddisfare l’insieme di dipendenze funzionali F, soddisfa anche un insieme di dipendenze più grande: $F^+$
$$F \subseteq F+$$
$F^+$ è la chiusura di F, cioè l’insieme di dipendenze funzionali che sono soddisfate da ogni stanza legale di R
# chiave e superchiave
dato uno schema R e un insieme di di dipendenze funzionali F, un sottoinsieme K dello schema R è una chiave di R se:
1. $K→R \in F^+$
2. non esiste un sottoinsieme **proprio** $K'$ di $K$ tale che $K’→R \in F^+$
con $K→R$ si intende che il sottoinsieme K determina l’intera istanza
se il sottoinsieme K soddisfa solo la proprietà 1, allora K è una superchiave di R.
per essere una chiave, la proprietà 2 deve essere soddisfatta (non devono esistere sottoinsiemi di K che sono a loro volta chiavi)
>[!example] Studente = Matr Cognome Nome Data
>- il numero di matricola viene assegnato allo studente per identificarlo,quindi
>- non ci possono essere 2 studenti con lo stesso numero di matricola,quindi
>- un’istanza di Studente, per rappresentare correttamente la realtà, non può contenere due tuple con lo stesso numero di matricola, quindi
$$\text{Matr} \to \text{Matr Cognome Nome Data}$$
deve essere soddisfatta da ogni istanza legale, quindi
**Matr è una chiave per Studente**

>[!warning] uno schema può avere più chiavi
# chiave primaria
anche se uno schema può avere più chiavi, in SQL una di esse verrà scelta come **chiave primaria**, e come tale non può assumere valore nullo

>[!info] per dichiarare le chiavi che non sono primarie su SQL, esistono le direttive UNIQUE e NON_NULL(tipo)

# dipendenze funzionali banali
dati uno schema R e due sottoinsiemi non vuoti, 
$$X,Y \subset R : Y \subseteq X$$
ogni istanza di R soddisfa la dipendenza funzionale 
$$X\to Y$$
>[!figure] ![[Pasted image 20241017074537.png]]

pertanto, se $Y \subseteq X$, allora $X \to Y \in F^+$
tale dipendenza è detta banale !
## proprietà di dipendenze funzionali
$$X\to Y\in F^+ \iff \forall A\in Y(X\to A\in F^+)$$
dato che $X \to Y$ deve essere soddisfatta da ogni istanza di R, si ha:
- Se $t_{1}[X]=t_{2}[X]$ allora $t_{1}[Y]=t_{2}[Y]$
- se $A \in Y$ e $t_{1}[A]\neq t_{2}[A]$, non può essere $t_{1}[Y]=t_{2}[Y]$
- se $\forall A \in Y, t_{1}[A]=t_{2}[A]$, allora avremo $t_{1}[Y]=t_{2}[Y]$ 
>[!figure] ![[Pasted image 20241017075818.png]]

ma come facciamo a calcolare l’insieme di dipendenze $F^+$ che viene soddisfatto da ogni istanza legale di uno schema R, su cui è definito un insieme di dipendenze funzionali R?
- abbiamo concluso banalmente che $F \subseteq F^+$, in quanto un’istanza è legale solo se soddisfa tutte le dipendenze di F, ma dobbiamo trovare le altre !
partiamo da un insieme diverso, “facile” da calcolare:
# $F^A$
$F^A$ in cui A sta per Armstrong
anche $F^A$ parte da F, e viene costruito in maniera ricorsiva applicando i 3 assiomi di Armstrong:
- se $f \in F$ allora $f \in F^A$
## assioma della riflessività
- se $Y \subseteq X \subseteq R$ allora $X → Y \in F^A$ (dipendenze banali)
>[!example] 
$Nome \subseteq (Nome, Cognome)$ ($X={Nome, Cognome}, Y={Nome) 
ovviamente se due tuple hanno uguale X, allora avranno sicuramente uguale l’attributo Nome(Y), e idem per Cognome.
quindi la dipendenza viene sempre soddisfatta

## assioma dell’aumento
- se $X→Y \in F^A$ allora, per ogni $Z \subseteq R$, $XZ → YZ \in F^A$ (assioma dell’aumento)
>[!example]
>$CodFiscale \to Cognome$ è soddisfatta se, quando 2 tuple hanno lo stesso CodFiscale, allora hanno lo stesso Cognome.
>Se la dipendenza è soddisfatta e aggiungo l’attributo indirizzo da entrambe le parti, avrò che se due tuple sono uguali in (CodFiscale, Indirizzo), quindi
>$t_{1}[CodFiscale]=t_{2}[CodFiscale] \land t_{1}[Indirizzo]=t_{2}[Indirizzo]$
allora sicuramente $t_{1[Cognome]=t_{2}[Cognome]\land t_{1}[Indirizzo]=t_{2}[Indirizzo]}$
>quindi se viene soddisfatta $CodFiscale \to Cognome$
>viene soddisfatta anche
>$CodFiscale, Indirizzo → Cognome, Indirizzo$
## assioma della transitività
- se $X→Y \in F^A$ e $Y→Z \in F^A$ allora $X→Z \in F^A$ (assioma della transitività)
>[!example]
>date $Matricola \to CodFiscale$ e $CodFiscale \to Cognome$, se entrambe le dipendenze sono soddisfatte, due duple cha hanno Matricola uguale avranno CodFiscale uguale, e due tuple che hanno CodFiscale uguale avranno Cognome uguale. quindi sarà soddisfatta anche $Matricola \to Cognome$

dimostreremo che $F^+ = F^A$, applicando gli assiomi ricorsivamente partendo da F