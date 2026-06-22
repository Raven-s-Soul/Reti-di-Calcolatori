# Il livello di trasporto e i protocolli TCP e UDP
il servizio offerto dallo strato di trasporto allo strato superiore deve essere affidabile ed è normalmente connesso
- nell’ambito di una connessione la trasmissione è bidirezionale contemporanea tra due parti
- le primitive di servizio dello strato di trasporto sono offerte ad una popolazione di utenti-programmatori molto ampia

### primitive di trasporto
- listen
- connect
- send
- receive
- disconnect

> i pacchetti scambiati nell’ambito di una connessione sono numerati, da entrambe le parti, in modo sequenziale
> - così da poter riscontrare esplicitamente l’avvenuta ricezione di uno o più pacchetti con degli ACK
>
> l’instaurazione è basata sulla scelta e sul relativo riscontro dei numeri iniziali di sequenza utilizzati per la numerazione dei pacchetti
> - three-way handshake

### three-way handshake
- C1 sceglie il numero di sequenza iniziale x per i propri
pacchetti
- C1 invia una Connection Request (CR) con x a C2
- C2 riceve la Connection Request con x
- C2 sceglie il proprio numero di sequenza iniziale y
- C2 invia una Connection Accepted riscontrando (ACK) x e proponendo il proprio numero iniziale y a C1
- C1 riceve la Connection Accepted da C2
- C1 riscontra (ACK) y a C2

> usare numeri diversi per ogni connessione consente di distinguere pacchetti che, pur scambiati dalla stessa coppia di interlocutori, magari in tempi diversi, appartengono a connessioni diverse

### rilascio di connessioni
se il rilascio è troppo rudimentale si possono perdere dei dati
- una possibile soluzione: il rilascio simmetrico
- in che istante la connessione può dirsi definitivamente rilasciata?
- in pratica ci si accontenta di tecniche analoghe al three-way handshake con timeout

## TCP
>rfc 793, 1122 e 1323

servizio di trasmissione dati affidabile bidirezionale contemporaneo (full duplex) punto-punto (host-to-host, end-to-end)
- TCP è il primo tra i protocolli studiati fino ad ora nella Internet Protocol Suite a garantire l'affidabilità del servizio di trasmissione

il servizio offerto da TCP è connesso e la connessione avviene fra due processi, identificati dall’indirizzo IP e da un numero di port da entrambe le parti

TCP specifica come distinguere più destinazioni (processi) su una stessa macchina
- i port sono i TCP sap
- un numero di port ha 2 byte
- i port < 1024 sono riservati per servizi standard (ad es. ftp = 20 e 21, telnet = 23, smtp = 25, http = 80)
   - si veda, ad es., Service Name and Transport Protocol Port Number Registry - IANA

### pacchetti e byte
- la t-pdu (pacchetto) TCP si chiama segment
- segment = header + dati (opzionali)
- limiti del segment
   - limite di 65535 byte per i dati IP
- TCP non numera i pacchetti ma i byte; ogni byte spedito ha un numero di sequenza a 32 bit
- i numeri di sequenza sono usati anche per gli ACK

### segment
i segment vengono usati per
- instaurare connessioni
- spedire dati
- spedire acknowledgement (ACK)
- chiudere connessioni

### segment – sequence number e ACK number
- il sequence number stabilisce la posizione del pacchetto dati nel flusso di informazioni del mittente, questi infatti trasferisce un flusso di dati generando un certo numero di pacchetti da inviare attraverso la rete
- l'acknowledgement (ACK) number viene utilizzato per riscontrare i byte ricevuti correttamente (significato: prossimo byte atteso)
- il sequence number si riferisce al flusso che va nella stessa direzione del segment mentre l’ ACK number si riferisce al flusso che va in direzione opposta al segment

### segment – hlen e code
hlen: numero di parole di 32 bit dell'intestazione

code determina il tipo di messaggio contenuto nel segmento – bit più importanti:
- bit significato
-URG urgent pointer field is valid
-ACK ACK field is valid
-PSH this segment requests a push
-RST reset the connection
-SYN synchronize sequence numbers
-FIN sender has reached end of its
-byte stream (rilascio)

> urgent pointer consente di stabilire dove si trovino eventuali dati urgenti presenti nel segment
>
> checksum interessa l'intero segment + gli indirizzi IP + protocol
>
> opzioni: la più utilizzata specifica la massima ampiezza del campo dati
>
> per ogni connessione, TCP ha un buffer (memoria disponibile, gestita come una coda) di trasmissione e un buffer di ricezione
>
> l’effetto di una primitiva send è quello di inserire i dati da spedire nel buffer di trasmissione
>
> l’effetto di una primitiva receive è quello di prelevare dati, già ricevuti da TCP, dal buffer di trasmissione
>
> window specifica la ampiezza corrente della finestra di controllo di flusso (l’orologio di TCP ricevente (da Wikipedia))
>

- per rilasciare la connessione una delle due parti manda un segment con FIN=1; non ha più dati da trasmettere
- quando la parte che ha mandato FIN=1 riceve in riscontro un ACK considera chiuso il suo flusso
- i dati possono continuare a fluire in direzione opposta
- la connessione è rilasciata quando entrambi i flussi sono chiusi
- il rilascio normale richiede 4 segment
- il primo ACK ed il secondo FIN possono essere nello stesso segment
- uso di timeout per il problema dei due eserciti; timeout = 2 * tempo di vita stimato di un pacchetto

### stato established
lo stato established corrisponde a molti stati più specifici, sia in ricezione
- riscontro dei pacchetti ricevuti correttamente
- gestione della finestra di controllo di flusso

## User Datagram Protocol – UDP
- UDP fornisce un servizio di trasmissione non connesso e non affidabile
- viene utilizzato da servizi nei quali:
   - non è importante che un pacchetto sia riscontrato (si preferisce l’efficienza rispetto all’affidabilità)
   - oppure si preferisce gestire il meccanismo dei riscontri a livello applicativo

- l'header è diviso in 4 campi di 16 bit che specificano il port da cui viene inviato il messaggio, il port destinazione, la lunghezza del datagram ed il checksum
- i port UDP sono analoghi ai port TCP
- a livello 3 può usare indifferentemente IPv4 o IPv6



