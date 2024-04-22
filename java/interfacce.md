le interfacce sono uno strumento che Java mette a disposizione per consentire a più classi di fornire e implementare un insieme di metodi comuni
esse definiscono e standardizzano l’interazione fra oggetti tramite un insieme limitato di operazioni
>[!tuff] LE INTERFACCE PERMETTONO DI MODELLARE COMPORTAMENTI COMUNI A CLASSI CHE NON SONO NECESSARIAMENTE IN RELAZIONE GERARCHICA !!(is-a, has-a)
>questa è l’utilità delle interfacce !!


esse specificano soltanto il comportamento (le classi astratte possono definire anche un costruttore) che un certo oggetto deve presentare all’esterno, cioè cosa quell’oggetto può fare. l’implementazione di tali operazioni, cioè come queste vengono tradotte e realizzate, rimane invece non definito
>[!tuff] no costruttori, no implementazioni. le interfacce sono classi astratte al 100% !!!


## dichiarazione di un’interfaccia
un’interfaccia è una classe che può contenere soltanto:
- costanti
- metodi astratti
- da java8: metodi di default e metodi statici
- da java9: metodi privati tipicamente da invocare in metodi di default
tutti i metodi dichiarati in un’interfaccia sono implicitamente **public abstact**
tutti i campi dichiarati in un’interfaccia sono implicitamente **public static final**
>[!tuff] tranne per i metodi di default o statici, non è possibile specificare alcun dettaglio implementativo !
>