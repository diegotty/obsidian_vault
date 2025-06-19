---
related to: 
created: 2025-03-02T17:41
updated: 2025-06-19T19:04
completed: true
---
# shell
la shell è un’interprete di comandi: un programma che esegue altri comandi
esistono vari tipi di shell: 
- Thompson/Bourne shell: sh
- Bourne-Again shell: bash
- KornShell: ksh
- zsh (extended and improved bash shell, con feature da ksh, bash, tcsh)
## comandi
le opzioni possono essere specificate in 2 modi:
- `-carattere` (vecchia versione) (es: `-r`)
- `--parola` (nuova versione) (es: `--recursive`)
le opzioni possono avere un argomento, specificato in modi diversi:
  - `--key=1`
  - `-k 1`
  - `-k1`
  - le opzioni senza argomento sono raggruppabili come `-b -r -c` che è equivalente a `-brc`
  - esistono anche le opzioni BSD-style, che sono senza dash come `tar xfz nomefile.tgz`
>[!example] comando `man`
`man` sta per manuale, e ci permette di avere più informazioni sul comando specificato come argomento
sinossi del comando `man`: `man [sezione] comando`
`man man`: 
![[Pasted image 20250304212204.png]]

# utenti

> [!warning] non tutti gli utenti possono fare login !
>  l’utente `root` non può fare login ma un utente può acquisire i diritti di `root` mediante i comandi `su` e `sudo`

- ogni utente appartiene almeno ad un gruppo
esistono molti gruppi definiti per scopi “amministrativi”
## su e sudo
utente **sudoer**: utente che appartiene al gruppo predefinito **sudo**
- gli utenti che appartengono al gruppo sudo possono eseguire comandi da `root` usando il comando `sudo`
il comando `su`(switch user ?) invece permette di cambiare utente
- `su -`, `su - root`, `su -l root` permettono di diventare root per il resto della sessione sulla shell ! 
