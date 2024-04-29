le classi usate finora vengono dette top-level, cioè esse si trovano più in alto di tutte le altre e non sono contenute in altre classi(e richiedono un file .java con lo stesso nome della classe contenuta nel file)
# classi annidate
sono classi all’interno di altre classi
possono essere 
- static 
- non-static(in questo caso vengono dette **classi interne**)
## perchè sono utili ? 
- raggruppamento logico delle classi 
	- se una classe è utile solo ad un’altra classe, è logico inserirla al suo interno e tenere le due classi **logicamente vicine**
- incrementa incapsulamento
	- una classe B annidata in A può accedere ai membri di A (anche se privati), ma B può essere nascosta al mondo esterno
- codice + leggibile e + facile da mantenere
	- la vicinanza spaziale è un fattore decisivo

### classi interne
- prima di poter creare un’istanza di una classe interna, è necessario istanzare la top-level che la contiene
- ciascuna classe interna ha un riferimento implicito all’oggett