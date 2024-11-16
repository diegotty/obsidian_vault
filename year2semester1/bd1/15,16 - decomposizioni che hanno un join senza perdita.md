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


>[!info] **b)** $\pi_{R_{i}}(m_{\rho}(r))= \pi_{R_{i}}(r)$

>[!info]  **c)** $m_{\rho}(m_{\rho}(r)) = m_{\rho}(r)$

**a)** $r \subseteq m_{\rho}(r)$
**b)** $\pi_{R_{i}}(m_{\rho}(r))= \pi_{R_{i}}(r)$
**c)** $m_{\rho}(m_{\rho}(r)) = m_{\rho}(r)$
