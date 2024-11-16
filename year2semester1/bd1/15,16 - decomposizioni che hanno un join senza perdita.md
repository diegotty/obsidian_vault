---
created: 2024-11-15
related to: "[[decomposizione]]"
updated: 2024-11-15, 19:30
---
come abbiamo visto nelle [[decomposizione#condizioni della decomposizione| condizioni per una decomposizione corretta]], l’ultima condizione, che non sappiamo anche verificare in una decomposizione, è che deve permettere di **ricostruire mediante join naturale** ogni **istanza legale dello schema originario**(senza aggiunta di informazione estranea)

inoltre, nell [[decomposizione|esempio 2 della decomposizione]] abbiamo visto cosa signfica, e come capita, di avere perdita di informazioni in una decomposizione
>[!info] definizione
>sia $R$ uno schema di relazione. Una decomposizione$\rho = \{R_{1}, R_{2}, \dots, R_{k}\}$ di $R$ ha un join senza perdita se **per ogni istanza legale $r$** di $R$ si ha $r=\pi_{R_{1}} \bowtie \pi_{R_{2}} \bowtie \dots \bowtie \pi_{R_{k}}(r)$
# teorema 
Sia $R$ uno schema di relazione e $\rho = \{R_{1}, R_{2}, \dots, R_{k}\}$ una decomposizione di $R$. per ogni istanza legale $r$ di $R$, le seguenti proprieta valgono per $r$ e $m_{p}$