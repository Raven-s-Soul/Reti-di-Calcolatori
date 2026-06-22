# il progetto IEEE 802
>[!TIP]
> definizione di un insieme di standard di livello fisico (livello 1) e di livello data-link (livello 2) per far comunicare computer sulla stessa rete locale (LAN), rete personale (PAN) o rete metropolitana (MAN)
>- gli standard riguardano tecnologie con pacchetti di lunghezza (numero di byte) variabile

>- 802.1 higher layer and management
>- 802.2 Logical Link Control (ormai consolidato)
>- 802.3 Ethernet
>   - 802.3u fast Ethernet
>   - 802.3z gigabit Ethernet
>   - 802.3ae, 802.3ak 10 gigabit Ethernet
>- 802.11 wireless LAN
>- 802.15 wireless PAN

>[!IMPORTANT]
>IEEE 803 distingue, nel livello 2, i sottolivelli MAC e LLC
>- i due sottolivelli corrispondono a due tipologie di pacchetti

## il sottolivello MAC - Media Access Control

>[!NOTE]
>- il MAC è specifico per ogni tipo di LAN
>- assunzione di partenza: i computer che devono comunicare sono posizionati nella stessa LAN; ciò richiede la soluzione di due problemi:
>   - (1) in ricezione, determinazione del destinatario (e del mittente)
>   - (2) in trasmissione, se la LAN è realizzata con un unico canale trasmissivo condiviso (es. IEEE 802.11), verifica della disponibilità del canale e soluzione di eventuali conflitti

> assunzione di partenza: i computer sono posizionati nella stessa LAN

>pdu = protocol data unit = trama = frame = pacchetto
>
>sap = service access point

### Ricezione

utilizzo di indirizzi MAC (presenti nella MAC pdu) che consentano trasmissione
- punto-gruppo (multicast): da un computer a un gruppo di computer sula LAN
- broadcast: da un computer a tutti i computer sulla LAN

## struttura delle MAC pdu
la MAC pdu ha vari campi
- i seguenti campi sono presenti in ogni tipo di tecnologia prevista dallo standard IEEE 802

|MAC-dsap|MAC-ssap|info(payload)||
|:-:|:-:|:-:|:-:|
|indirizzo del destinatario| indirizzo del mittente|LLC pdu| FCS |

FCS = Frame Check Sequence, per l’identificazione di eventuali errori

- indirizzi di 6 byte
- di solito rappresentati con notazione esadecimale
   - ogni gruppo di 4 bit è rappresentato da una cifra compresa tra 0 e F
   -  i byte sono separati tra loro da due punti o da trattini
   -  es: FF:11:1A:2B:A2:9S[^1] è un indirizzo MAC

>[!WARNING]
>- unicast: identificano le singole schede di rete dei computer
>   - se l’ultimo bit del primo byte ha valore zero, allora l’indirizzo MAC è unicast
>- multicast: identificano gruppi di schede di rete
>   - se l’ultimo bit del primo byte ha valore uno, allora l’indirizzo MAC è multicast
>- broadcast: tutte le le schede di rete
>   - l’indirizzo MAC broadcast è FF:FF:FF:FF:FF:FF

>- lo standard che stabilisce il formato degli indirizzi MAC si chiama EUI-48 (Extended Unique Identifier)
>- gli indirizzi sono forniti alle schede di rete dei computer in modo tale da essere unici a livello mondiale
>  - i primi 3 byte sono assegnati al costruttore (Organization Unique Identifier)
>  - i secondi 3 byte sono definiti dal costruttore


### Trasmissione
(2) in trasmissione, se la LAN è realizzata con un unico canale trasmissivo condiviso (es. IEEE 802.11), verifica della disponibilità del canale e soluzione di eventuali conflitti


## il sottolivello LLC - struttura delle LLC pdu
|LLC-dsap|LLC-ssap|control|info(payload)|
|:-:|:-:|:-:|:-:|
|indirizzo del destinatario| indirizzo del mittente|tipo pdu| pdu livello 3 |

> attribuiti da IEEE solo ai protocolli (ufficialmente) standard
> - se gli LLC-sap hanno valore AA, allora il pacchetto contiene un pacchetto di protocollo non standard







[^1]: Ester egg from Nier.
