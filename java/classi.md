# def
Le classi rappresentano stati/concetti/entità, e sono un prototipo astratto per gli oggetti di un tipo(tipo della classe)
## Struttura
- Campi(stati)
- Metodi(comportamenti)
> [!Warning] Il nome di una classe inizia sempre con la maiuscola e sono case sensitive

***
# packages
Le classi sono organizzate in packages. Alcuni dei package più comuni sono:
- java.lang - package speciale che contiene le classi fondamentali(es. String, System, etc)
- java.util - classi di utilità
- java.awt - 4 graphics and windows
- javax.swing, javafx - 4 gui
***
# campi
costituiscono la memoria privata di un oggetto ( sono [[private]] la maggior parte delle volte)
dichiarazione di un campo:
```java
private [final] [static] tipo_di_dati nome;
```
## final
un campo final è una costante(dopo il primo valore assegnatogli)
## static ( campo di classe)
un campo static è condiviso da tutti. è allocato una sola volta in memoria per tutti gli oggetti
***

# costruttore
serve ad ottenere un oggetto istanza della classe. Ci possono essere più costruttori !
non hanno valori di uscita, ma non specificano void.
una classe può avere più costruttori(che differiscono nel numero e tipo dei parametri)
java crea per ogni classe un costruttore di default vuoto se non creato manualmente
> [!tuff]
 i campi di una classe vengono inizializzati con dei valori di default !!


***
## metodi
they do things
## overloading
```java
public void reset() {value = 0}

public void reset(int NewValue) {value = newValue}
```
***
# visibilità
## occultatori di visibilità
- private: le cose dichiarate come private non possono essere viste da altre classi(information hiding)
- public
- protected
## incapsulamento
semplifica il lavoro di sviluppo.
funzionamento a "scatola nera":
- non è necessario conoscere i dettagli implementativi delle classi con cui si interagisce
facilita il lavoro di gruppo e l'aggiornamento del codice
una classe interagisce quindi con le altre quasi solo attraverso costruttori e metodi pubblici

i metodi di una classe possono chiamare i metodi pubblici e privati della stessa classe, ma solo i metodi pubblici di altre classi !!!