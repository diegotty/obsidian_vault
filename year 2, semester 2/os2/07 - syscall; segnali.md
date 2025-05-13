---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-13T08:59
completed: false
---
# segnali
i segnali sono **interrupt software**, e possono essere inviati:
- dal kernel ad un processo
- da un processo ad un altro processo
vengono spesso generati dopo condizioni anomale che si verificano, come:
- richiesta di sospensione da parte dell’utente (come prendendo con un processo running sul terminale`ctrl+z` = SIGSTOP)
- richiestsa di terminazione da parte dell’utente (`ctrl+c` = SIGINT)
- un’eccezione hardware (divisione per 0, riferimento di memoria non valido, etc… )
ma può essere anche generato da condizioni **non** anomale:
- la terminazione di un processo figlio
- un timer settato con a