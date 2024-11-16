---
created: 2024-11-15
related to: "[[decomposizione]]"
updated: 2024-11-15, 19:30
---
come abbiamo visto nelle [[decomposizione#condizioni della decomposizione| condizioni per una decomposizione corretta]], l’ultima condizione, che non sappiamo anche verificare in una decomposizione, è che deve permettere di **ricostruire mediante join naturale** ogni **istanza legale dello schema originario**(senza aggiunta di informazione estranea)

inoltre, nell [[decomposizione|esempio 2 della decomposizione]] abbiamo visto cosa signfica, e come capita, di avere perdita di informazioni in una decomposizione
>[!info] definizione
>sia $R$ uno schema di relazione. Una decomposizione$\rho = \{R_{1}, R_{2}, \dots, R_{k}\}$ di $R$ ha un join senza perdita se **per ogni istanza legale $r$** di $R$ si ha $r=\pi_{R_{1}}(r) \bowtie \pi_{R_{2}}(r) \bowtie \dots \bowtie \pi_{R_{k}}(r)$

>[!warning] ricordiamo che per $\pi_{R_i}(r)$, si intende la proiezione di $r$ sugli attributi presenti in $R_i$
# teorema 
Sia $R$ uno schema di relazione e $\rho = \{R_{1}, R_{2}, \dots, R_{k}\}$ una decomposizione di $R$. per ogni istanza legale $r$ di $R$, le seguenti proprieta valgono per $r$ e $m_{\rho}(r)=\pi_{R_{1}}(r) \bowtie \pi_{R_{2}}(r) \bowtie \dots \bowtie \pi_{R_{k}}(r)$ (join naturale delle proiezioni di $r$ su ogni sottoschema di $\rho$):
>[!info] **a)** $r \subseteq m_{\rho}(r)$
**dimostrazione**
sia $t$ una tupla di $r$. $\forall i, i=1,…,k, \,\,t[R_{i}] \in \pi_{R_{i}}(r)]$, e quindi $t \in m_{\rho}(r)$
cioè:
>- il sottoinsieme dei suoi valori su attributi di $R_i$ sarà sicuramente in una tupla della proiezione $\pi_{R_{i}}(r)$
>- quindi **OGNI** sottoinsieme dei valori di $t$ corrispondente agli attributi presenti in un sottoschema $R_i$ della decomposizione di $R$, comparirà in $\pi_{R_{i}(r)}$
>- inoltre sappiamo che se ci dovessero essere duplicati del sottoinsieme dei valori di $t$ in $\pi_{R_i}(r)$, tali valori apparirebbero solo una volta in $R_i$, in quanto la proiezione segue le regole insiemistiche ed elimina i duplicati
>- quindi ogni tupla potrà essere ricostruita tramite il join naturale, in quanto per ogni sottoinsieme di tale tupla, esiste un sottoschema che contiene i suoi valori
>- anzi, il join naturale potrebbe ottenere tuple che non erano in r ricomponendo pezzi di tuple diverse

>[!info] **b)** $\pi_{R_{i}}(m_{\rho}(r))= \pi_{R_{i}}(r)$
**dimostrazione**
per **a)**, si ha che $r \subseteq m_{\rho}(r)$, e quindi $\pi_{R_{i}}(r) \subseteq \pi_{R_{i}}(m_{\rho}(r))$
è sufficiente quindi dimostrare che $\pi_{R_{i}}(m_{\rho}(r))\subseteq \pi_{R_{i}}(r)$ (uguaglianza di insiemi dimostrata con doppia inclusione)
- per ogni tupla $t \in m_{\rho}(r)$ , e per ogni $i,i=1,…,k$, deve esistere un tupla $t’ \in r$ tale che $t[R_i]=t'[R_{i}]$. in quanto, se tale tupla $t’$ non esistesse, non troveremmo i valori di $t$ su $R_i$ nella proiezione $\pi_{R_{i}(r)}$, e di conseguenza non li avremmo nel join naturale

>[!info]  **c)** $m_{\rho}(m_{\rho}(r)) = m_{\rho}(r)$
per **b)** si ha $\pi_{R_{i}}(m_{\rho}(r))= \pi_{R_{i}}(r)$
pertanto, applicando la definizione dell’operatore $m_{\rho}$ (join delle proiezioni), avremo:
$$m_{\rho}(m_{\rho}(r))= \pi_{R_{1}}(m_{\rho}(r)) \bowtie \pi_{R_{2}}(m_{\rho}(r)) \bowtie \dots \bowtie \pi_{R_{k}}(m_{R_{k}}(r)) =$$ 
$$\pi_{R_{1}}(r) \bowtie \pi_{R_{2}}(r) \bowtie \dots \bowtie \pi_{R_{k}}(r) = m_{\rho}(r)$$

# verifica di $\rho$ senza perdita
```pseudo
	\begin{algorithm}
	\caption{verifica che una decomposizione abbia join senza perdita}
	\begin{algorithmic}
	\Input uno schema di relazione $R$, un insieme di dipendenze funzionali $F$ su $R$, una decomposizione $\rho = \{R_1, R_2, ..., R_k\}$ di $R$
	\Output tabella per decidere se $\rho$ ha un join senza perdita
	\State costruire una tabella \textbf{r} di $|R|$ colonne e $|\rho|$ righe, dove:
	\State all'incrocio della i-esima riga e della j-esima colonna inserire $a_j$ se l'attributo $A_j \in R_i$, altrimenti $b_{ij}$
	\\
	\textbf{do}
	\For{ \textbf{every} $X \to Y \in F$}
	\If{$\exist$ due tuple $t_1, t_2$, t.c. $t_1[X]=t_2[X] \land t_1[Y]\neq t_2[Y]$}
	\State $t_2[A_J] := t_1[A_j]$
	\Else 
	\State $t_1[A_j]=t_2[A_j]$
    \EndIf
	\State
    \EndFor
	\State
	\textbf{while} $r$ non ha una riga con tutte '$a$' $\land$ $r$ è cambiato
	\If{$r$ ha una riga con tutte '$a$'}
	\State $\rho$ ha un join senza perdita
	\Else
	\State $\rho$ non ha un join senza perdita
    \EndIf
	\end{algorithmic}
	\end{algorithm}
```
- la tabella viene costruita con questa logica: ogni colonna è un’attributo dello schema $R$, e ogni riga rappresenta un sottoschema di $\rho$. all’inizio, inseriamo $a_j$ se l’attributo $A$ nella colonna $j$-esima appartiene all sottoschema $i$-esimo ($R_i$).
