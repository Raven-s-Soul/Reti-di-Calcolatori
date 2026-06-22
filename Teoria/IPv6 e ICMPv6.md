# IPv6 e ICMPv6

>IPv6 ha uno spazio di indirizzamento molto più ampio di IPv4
>- indirizzi con 128 bit invece di 32 bit
>- IPv6 sta rimpiazzando progressivamente IPv4

IPv4 e IPv6 sono tra loro alternativi
- sono entrambi protocolli di livello 3
- i pacchetti IPv4 e i pacchetti IPv6 spediti o ricevuti da un es/is sono trattati separatamente

## header IPv6
- 40 byte – spariscono le options

### version – 4 bit
- 6 – IPv6

### traffic class – 8 bit
- simile al type of service IPv4

### flow label – 20 bit
- per distinguere un flusso
-  pacchetti appartenenti allo stesso flusso hanno:
   - stesso indirizzo IPv6 sorgente, stesso indirizzo IPv6 destinazione, stesso valore del campo flow-label

### payload length – 16 bit
- specifica la lunghezza del campo dati nel pacchetto

### next header – 8 bit
- simile a protocol IPv4; consente anche di specificare options

### hop limit – 8 bit
- uguale al ttl IPv4

### source address – 16 byte
### destination address – 16 byte


### No more
- i campi per la frammentazione
   - la frammentazione avviene solo sugli es 
- checksum
   - si suppone che i controlli di livello 2 e di livello 4 siano sufficienti 
- hlen
   - header è di lunghezza fissa e ha sempre 40 byte 

## rappresentazione 
IPv4 si rappresenta con 4 numeri decimali separati da punti
- `xxx.xxx.xxx.xxx`
  
IPv6 si rappresenta con 8 numeri esadecimali separati da due punti 
- `ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff`
- `1234:0000:0033:0003:ffff:0000:0000:ffff` $\to$ `1234:0:33:3:ffff:0:0:ffff` $\to$
- `1234::33:3:ffff:::ffff` non va bene troppi `::`, il massimo è 1 `::`
- `1234:5678:9ABC:DEF0:0:0:0:0` $\to$ `1234:5678:9ABC:DEF0::`
- `0:0:0:0:0:0:0:1` $to$ `::1` è l'interfaccia di loopback della macchina
- `0:0:0:0:0:0:0:0` $to$ `::` 
- prefissi / uguali come in ipv4
- non ci sono prefissi più lunghi di una /64
- una netmask non è rappresentata nel formato in cui sono espliciti i valori dei suoi bit

>[!NOTE]
> - anche in IPv6 ci sono indirizzi unicast e multicast, ma non ci sono indirizzi broadcast
> - indirizzi unicast più importanti
>    - indirizzi global unicast, utilizzabili in Internet
>    - indirizzi link-local (LAN)
> - indirizzi multicast più importanti
>    -  solicited node, ff02::1 (tutte le macchine della LAN), ff02::2 (tutti router della LAN) 

### formato degli indirizzi unicast
un indirizzo unicast è sempre formato da due parti
- i primi 64 bit sono la parte prefisso, tipicamente usata per l’instradamento
- i secondi 64 bit, chiamati interface identifier (interface id), sono usati per identificare un’interfaccia

### indirizzi global unicast
- sono gli unici utilizzabili in Internet
- sono assegnati dai registri internazionali

### indirizzi link-local
- hanno prefisso fe80::/64
- sono utilizzabili solo all’interno della LAN
   - nel gergo IPv6, LAN e link sono sinonimi
- non sono utilizzabili in Internet

### attribuzione degli indirizzi a una interfaccia
- un’interfaccia di rete può avere vari indirizzi IPv6
- come per IPv4, anche in IPv6 gli spazi di indirizzamento global unicast attribuiti a due LAN diverse devono essere disgiunti
- gli indirizzi possono essere attribuiti alle interfacce con configurazione manuale o in modo automatico

> con l’autoconfigurazione stateless
> - l’interfaccia si auto-attribuisce, senza l’intervento di un amministratore di rete, un indirizzo link-local
> - l’interfaccia inoltre, collaborando con i router, si attribuisce eventualmente anche uno o più indirizzi global unicast
>
> si compone delle fasi seguenti
> - l’interfaccia si auto-attribuisce automaticamente un interface id
> - sulla base dell’interface id, l’interfaccia si attribuisce un indirizzo link-local
> - dialogando con i router presenti sulla propria LAN, l’interfaccia ottiene uno o più prefissi da concatenare con il proprio interface id, quindi si attribuisce uno o più indirizzi global unicast e sceglie il/i router di default

#### auto-attribuzione di un interface id
la scelta dell’interface id può essere fatta in modo randomico o sfruttando il proprio indirizzo MAC

## dall’indirizzo MAC all’interface id
- l’interfaccia divide il proprio indirizzo MAC (di 48 bit) in due parti, ciascuna di 24 bit
- tra le due parti (al centro dell’indirizzo MAC) inserisce la sequenza di 16 bit ff:fe
- poi attribuisce valore 1 al settimo bit della prima parte
   -  nell’indirizzo MAC quel bit vale 0 se l’indirizzo è unico a livello mondiale e 1 altrimenti

## scelta di un indirizzo link-local

- un’interfaccia costruisce il proprio indirizzo link-local anteponendo al proprio interface id il prefisso fe80::/64
- prima di considerare tale indirizzo come indirizzo corretto, l’interfaccia svolge un’attività di Duplicate Address Detection (DAD) nella propria LAN

### acquisizione di prefissi da usare in Internet

- i router inviano periodicamente su ogni LAN a cui sono connessi dei pacchetti di router advertisement
   - annuncia la propria presenza
   - fornisce un elenco di prefissi utilizzabili sulla LAN e il loro tempo di validità, specifica se è disponibile a fare da router di default
   - fornisce alle interfacce altre informazioni utili
   - fornisce il proprio indirizzo MAC
- le interfacce possono sollecitare pacchetti di router advertisement con pacchetti di router solicitation

## spedizione di un pacchetto e IP lookup

la spedizione di un pacchetto da parte di un es avviene come in IPv4
- determinazione del fatto che il destinatario sia sulla stessa LAN o no

le tabelle di instradamento dei router hanno lo stesso significato delle tabelle IPv4 e sono usate allo stesso modo

#### per IPv6 non c’è ARP

## ICMPv6
è l’evoluzione di ICMP per IPv6
- svolge le stesse funzioni di ICMP (messaggistica di errore, ecc.), alle quali si aggiungono le funzioni di altri protocolli, tra i quali ARP

### come trovare l’indirizzo MAC di un sistema
- spedizione di un pacchetto ICMPv6 a un gruppo multicast
- quando a un sistema viene assegnato un indirizzo IPv6 il sistema assume anche di appartenere ad uno specifico gruppo multicast

>#### gruppo multicast solicited node
>l’indirizzo multicast solicited node di un sistema è fatto come segue, dove i 24 bit IID sono gli ultimi 24 bit del suo indirizzo IPv6

per ottenere un indirizzo MAC di un sistema, il nodo calcola l’indirizzo multicast solicited-node corrispondente all’indirizzo IPv6 del destinatario
- il nodo invia a questo indirizzo un pacchetto ICMPv6 di neighbor solicitation specificando l’indirizzo IPv6 del destinatario nel campo dati

- il destinatario, se presente, risponde con un pacchetto unicast di neighbor advertisement
- il suo indirizzo MAC è specificato nella parte dati del pacchetto
- viene memorizzato nella neighbor cache

> #### perché usare solicited node?
> - perché, mentre un pacchetto broadcast è considerato da tutte le schede della LAN come ad esse destinato, un pacchetto multicast è considerato come ad esse destinato dalle sole schede di quel gruppo multicast
> - visto che l’indirizzo solicited node contiene gli ultimi bit dell’indirizzo è probabile che venga processato da poche schede

### esempio di ricerca di un indirizzo MAC
...
