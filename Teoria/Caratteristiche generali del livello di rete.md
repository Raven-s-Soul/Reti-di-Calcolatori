# Caratteristiche generali del livello di rete
> il livello di rete sceglie una strada per i pacchetti
>
> la strada verso la destinazione è composta da vari passaggi intermedi (salti o hop)
>
> il livello di rete fornisce servizi al livello di trasporto, il quale non è interessato a conoscere:
> - il numero e la topologia delle varie reti
> - le tecnologie di livello inferiore (livello 2) usate

> i servizi offerti dal livello di rete al livello di trasporto possono essere connessi o non connessi
> - più diffuso (IPv4) offre esclusivamente un servizio non connesso
>
> l’instradamento può essere a datagramma o a circuito virtuale
> - un livello di rete connesso fa tipicamente uso della commutazione a circuito virtuale
> - un livello di rete non connesso fa tipicamente uso della commutazione a datagramma

### interfaccia con il livello 4
>il livello di trasporto (livello 4) ha bisogno di conoscere le macchine attraverso:
>- indirizzi univoci, distribuiti in modo consistente su tutta la rete

### assunzioni e terminologia
>dal punto di vista del livello 3 i sistemi sulla rete possono essere:
>- end system (es) o intermediate system (is)
>
>ogni sistema è associato un indirizzo numerico
>- talvolta, oltre all'indirizzo, al sistema è associato un nome, informazioni sulla corrispondenza nomi-indirizzi sono gestite da server appositi, spesso chiamati name server
>
>gli is, chiamati anche router o gateway, operano a livello 3 e contengono (almeno) gli strati 1, 2 e 3 dell’architettura iso-osi

## indirizzamento

- indirizzo di livello 2 (MAC) identifica il destinatario in una LAN
- l’indirizzo di livello 3 identifica il destinatario nell’ambito dell’intera rete
- un sistema necessita di tanti indirizzi MAC quante sono le sue schede di rete e di un solo indirizzo di livello 3

## metodi di instradamento

### routing by network address
- nel pacchetto c’è l’indirizzo del sistema destinatario
- la commutazione è a datagramma in base a tale indirizzo

>l’is usa l'indirizzo come chiave di ricerca in una tabella locale e determina la linea di ritrasmissione

### label swapping
- nel pacchetto c’è una label che identifica il cammino virtuale
- la commutazione è a circuito virtuale in base alla label

> ((senza) swapping) ogni is usa la label come chiave di ricerca in una tabella locale, molto piccola, e determina la linea di ritrasmissione
>
> la tabella di un is che fa label swapping contiene l’elenco dei circuiti virtuali che lo attraversano in un certo istante
> - per evitare di dover verificare nell’intera rete che una specifica label non sia già utilizzata per qualche altra connessione, ad ogni tratto del circuito è assegnata una label (in generale) diversa ed assegnata localmente

### source routing
- nel pacchetto è specificata la lista ordinata di tutti gli is da attraversare per arrivare a destinazione

> nel pacchetto tutta la strada da fare tra A e B
