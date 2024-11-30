---
created: 2024-11-30
related to: 
updated: 2024-11-30T17:53
---
# insiemistica
**partizione**: sia $\mathcal{P} \subset \mathcal{P}(X)$, $\mathcal{P}$ è una partizione di $X$ se:
1. $\forall P \in \mathcal{P}, P \neq \varnothing$
2. $\forall P, Q \in \mathcal{P}, P \neq Q \implies P \cap Q = \varnothing$
3. $\forall x \in X, \exists P \in \mathcal{P} \,\,t.c. \,\ x \in P$
classi di equivalenza sono una partizione 
**insieme quoziente**: insieme delle classi di equivalenza di un insieme per una relazione
**sistema completo di rappresentanti**: selezione di elementi che rappresentano ogni classe di equivalenza di una relazione di equivalenza(un solo elemento per classe di equivalenza, per ogni classe di equivalenza)

la composizione è associativa ! (in insiemistica) $(h\circ g) \circ f = h \circ (g \circ f)$
una funzione è biettiva se esiste una funzione inveresa che, composta alla funzione inziale, crea una funzione identità

**anello commutativo unitario**: sestupla $(A,+,-,\cdot,0_A, 1_A)$
i cui dati devono soddisfare le seguenti condizioni:
1. $\forall a,b, \in A, a+b = b+a$
2. $\forall a,b,c \in A, (a+b)+c = a+(b+c)$
3. $\forall a\in A, a+0_{A}=0_{A}+a=a$
4. $\forall a \in A, a +(-a) = 0_{A}$
5. $a(bc)=(ab)c=abc$
6. $a(b+c)=ab+ac, (a+b)c=ac+bc$
7. $a \cdot 1_{A}=1_{A} \cdot a=a$
8. $ab=ba$

tricotomia:$\forall a \in Z$ si ha che $a \in N^*$, oppure $a=0$, oppure $-a ‘ si hac che $a9 \in N^*$, oppure $a=0$, oppure $-a \in N^*$. la tricotomia ci permette di stabilire una relazione di ordinamento tra gli elementi di un anello

**elementi invertibili**: dato $A$ anello con $1_A \neq 0_A$, $a \in A$ è invertibile se $\exists \,b \in A \,\,t.c.\, ab=ba=1_A$. $A^{\times}$ è insieme degli invertibili
- se $a$ è invertibile, $a^{-1}$ è unicamente determinato !
**relazione di divisibilità**: dato $A$ anello e $a,b \in A$, $a|b \iff \exists c \in A : b=ac$
- è riflessiva e transitiva
altre proprietà della relazione di divisibilità: 
- se $a|b \land a|c \implies a|b+c$

**elementi irriducibili**: $a \in A/A^{\times}$ è detto irriducibile se $\forall b,c \in A : a=bc$ allora $b \in A^{\times}$ oppure $c \in A^{\times}$
**elementi primi**: $a \in A/A^{\times}$, $a \neq 0$, è detto primo se $\forall b,c \in A$, se $a|bc$ allora $a|b$ oppure $a|c$