# IPv4, ARP e la suite TCP-IP

il protocollo IPv4 (Internet Protocol versione 4) è il principale protocollo di rete (di livello 3) della "suite TCP/IP", detta anche "Internet Protocol Suite"
- la suite TCP/IP è la pila protocollare usata in Internet ed è implementata nella stragrande maggioranza dei computer
- i protocolli della suite sono descritti da documenti chiamati Request For Comments (RFC)

## Story
- prima metà degli anni ’70: la defense advanced research project agency (darpa) finanzia l’università di Stanford e la BBN per sviluppare protocolli per lo scambio di dati tra computer
- fine degli anni ’70: nascono l’Internet Protocol Suite con Transmission Control Protocol (TCP) e Internet Protocol (IPv4) e una prima versione di Internet chiamata arpanet

### Architettura
- HTTP/TCP/IPv4/802.3
- SMTP/TCP/IPv4/802.3
- SNMP/UDP/IPv4/802.11
  
> sotto lo strato IPv4 ci possono essere i vari standard LAN (es. IEEE 802) e WAN (es. ppp)

### Suite TCP/IP
- semplificazione estrema del data plane degli is e spostamento di tutte le funzioni complesse sugli es
- routing by network address, commutazione di pacchetto a datagramma
    - servizio offerto al livello 4 non connesso
- servizio best-effort
   - Il pacchetto può non essere consegnato
 
## IPv4 (Internet Protocol versione 4)
>- è attualmente, assieme a IPv6, il protocollo principale di liv. 3
>   - RFC 791
>- anche chiamato IPv4 datagram
>- funzioni di instradamento, frammentazione, riassemblaggio, rilevazione di errori
>- riceve messaggi dai protocolli UDP e TCP del livello superiore
>- provvede all’instradamento dei datagram, eventualmente frammentandoli
>- la dimensione del datagram dipende dai vincoli del livello due
>- provvedere al routing
>- lo strato IPv4 di un router non può comunque riassemblare i datagrammi

### formato dell’IPv4 datagram

analogamente ai pacchetti di livello 2, il datagram IPv4 è diviso in due parti: l’header e i dati

|datagram header|data area|
|:-:|:-:|

- l’header contiene gli indirizzi del mittente e del destinatario

### version (4 bit)
- è il numero di versione del protocollo IP che ha generato il pacchetto, attualmente vale 4

### ihl – internet header length (4 bit)
- è la lunghezza dell’header, espressa in parole da 32 bit (4 byte)
- valore minimo è 5

### type of service
il campo type of service specifica la priorità di un datagram rispetto a un altro

il campo è suddiviso in due parti:
- DSCP – Differentiated Services Code Point – in funzione del suo valore il pacchetto riceve un diverso trattamento
- ECN – Explicit Congestion Notification

> perché frammentare un pacchetto a livello tre?
> - ogni livello due attraversato potrebbe avere una maximum transmission unit (mtu, dimensione massima di un pacchetto) diversa

#### frammentazione e riassemblaggio
- consiste nella suddivisione (detta frammentazione) di un pacchetto in pacchetti più piccolo
- il pacchetto originale viene ricostituito (riassemblato) sulla macchina destinazione
- occorrono degli appositi campi nell’header di livello tre che consentano di:
   -  riconoscere il pacchetto come frammento
   -  riconoscere i frammenti generati dallo stesso pacchetto
   -  determinare l’ordine dei frammenti in maniera da poter riassemblare il pacchetto originale


### ident (16 bit)
- due datagrammi con lo stesso ident vengono riassemblati nello stesso pacchetto

### fragment offset (13 bit)
- serve a riunire i frammenti nel giusto ordine
- essendo di 13 bit, può specificare solo un offset multiplo di 8 (Convenzione ultimi 3 bit siano 0)
- tutti i frammenti eccetto l’ultimo hanno un numero di byte multiplo di 8; al massimo 8192 frammenti per datagramma

### flags (3 bit)
- il primo bit è riservato e deve essere sempre zero
- DF (don’t fragment) specifica se il datagram può essere frammentato, e può essere usato per eseguire una path mtu discovery molto primitiva
- MF (more fragment) permette di riconoscere l'ultimo frammento del pacchetto

### total length (16 bit)
- è la lunghezza del pacchetto completo di header e payload (al massimo 65.535 byte)

### time to live (8 bit)
- specifica qual è il tempo di vita del pacchetto
- decrementato con il passare del tempo
- quando vale 0 il pacchetto viene scartato
- è decrementato di una unità ad ogni “hop”
- quando il pacchetto è scartato il router manda un avvertimento al mittente

### protocol (8 bit)
- identifica il protocollo di livello superiore che è nel payload del pacchetto IPv4
- i possibili valori per protocol sono definiti da IANA
- 1 = ICMP; 6 = TCP; 17 = UDP; ...

### header checksum (16 bit)
- riguarda solo l’header; deve essere ricalcolato ad ogni hop.

### il campo options
è usato per informazioni meno frequenti nei pacchetti e per sperimentazione; ogni opzione inizia con 1 byte di codice che ne specifica il tipo
- sicurezza
- source routing
- loose s.rout.
- record route
- timestamp

## indirizzamento IPv4
gli indirizzi sono associati alle schede di rete e non agli host

spazio degli indirizzi:
- 32 bit (4 byte)
- univoci a livello mondiale
- 4 numeri decimali separati da punti (es 192.168.1.67)
- spesso agli indirizzi corrispondono dei nomi simbolici

>[!NOTE]
> problema: se gli indirizzi IPv4 fossero assegnati alle macchine in modo del tutto arbitrario allora la tabella di instradamento di un router dovrebbe contenere un numero enorme di righe
>
> idea generale di soluzione: assegnare indirizzi che cominciano nello stesso modo (con lo stesso prefisso) a macchine allocate fisicamente nella stessa rete locale
>- un is contiene quindi tabelle di prefissi invece di tabelle di indirizzi
>
>gli indirizzi sono quindi raggruppati in net (192.168.1.0 dove 0 non è assegnato a nessuno è indica l'intera net)
>- LAN = Net
>- semplificando le tabelle d’instradamento a una net invece di singole macchine

>un indirizzo IPv4 ha due funzioni
>-  identificazione: ogni sistema ha un indirizzo IPv4 distinto
>-  location: indirizzi con lo stesso prefisso sono fisicamente nella stessa rete locale

## Classi
nei primi anni di Internet sono state definite cinque classi di indirizzi con prefissi di diversa lunghezza
-  Class A, B, C, D, E

dato un indirizzo si poteva inferire la sua classe di appartenenza esaminandone solo i primi bit

#### A
|0|prefisso|hostid|
|:-:|:-:|:-:|
|0|1-7|8-31|
```
Da 0.0.0.0 a 127.255.255.255
```
#### B
|1|0|prefisso|hostid|
|:-:|:-:|:-:|:-:|
|0|1|2-15|16-31|
```
Da 128.0.0.0 a 191.255.255.255
```
#### C
|1|1|0|prefisso|hostid|
|:-:|:-:|:-:|:-:|:-:|
|0|1|2|3-23|24-31|
```
Da 192.0.0.0 a 223.255.255.255
```
#### D
|1|1|1|0|multicast address|
|:-:|:-:|:-:|:-:|:-:|
|0|1|2|3|4-31|
```
Da 224.0.0.0 a 239.255.255.255
```
#### E
|1|1|1|1|0|usi futuri|
|:-:|:-:|:-:|:-:|:-:|:-:|
|0|1|2|3|4|5-31|
```
Da 240.0.0.0 a 247.255.255.255
```

***
### Netmask

>[!TIP]
>per ragioni di flessibilità l’indirizzamento classful è stato abbandonato per consentire una lunghezza arbitraria della parte prefisso dell’indirizzo IPv4 (indirizzamento classless)
>- nell’indirizzamento classful la lunghezza del prefisso è univocamente determinata dal valore dell’indirizzo
>- nell’indirizzamento classless la lunghezza del prefisso deve essere indicata esplicitamente per ogni indirizzo
>
> ad ogni indirizzo IPv4 viene associata una netmask
> - sequenza di 32 bit, composta da uno contigui seguiti da zeri
> - specifica la lunghezza del prefisso (gli uno) e quella di hostid (gli zeri)
> - esempio (255.255.255.0) ovvero (11111111.11111111.11111111.00000000)
>
>la netmask 255.255.255.0 associata, a xxx.xxx.xxx.yyy indica che il prefisso è costituito dai primi tre
>byte e l'host è identificato dall'ultimo byte (yyy)
>- tutti gli indirizzi con lo stesso prefisso hanno la stessa netmask, quindi si può parlare di netmask di un prefisso
>- la netmask di un prefisso si può denotare più semplicemente specificando, dopo l’indirizzo IPv4, la lunghezza del prefisso, separata da una barra
>    - 192.168.1.0/24
>    - 192.168.1.0 con netmask 255.255.255.0

>due indirizzi IPv4 che hanno prefisso diverso (cioè sono in due diverse reti fisiche) se messi in and bit-a-bit con la propria netmask producono valori diversi

mettendo in and-bit-a-bit il proprio indirizzo e quello del destinatario con la propria netmask, l’es determina se un destinatario è locale o remoto
- se il destinatario è locale il pacchetto viene inviato al destinatario stesso (trasmissione diretta)
- se il destinatario è remoto il pacchetto viene inviato al default gateway della LAN, cioè ad un router (un is)

## IP lookup
ricerca nella tabella di instradamento (IP lookup)
- ogni riga della tabella di instradamento di un router contiene un prefisso (identifica una rete fisica - LAN) e la relativa netmask
- determina se l’indirizzo è nella LAN relativa (matching)
- la tabella viene controllata una riga dopo l’altra

|indirizzo|netmask|linea di inoltro|
|:-:|:-:|:-:|
|192.168.1.0|255.255.255.192|L1|
|123.2.1.0|255.255.255.128|L2|
|123.2.1.128|255.255.255.128|L3|
|67.4.39.0|255.255.255.128|L4|
|0.0.0.0|0.0.0.0|L5|

- 192.168.1.10 & 255.255.255.192 = 192.168.1.0 $\to$ L1
- 123.2.1.12 & 255.255.255.128 = 123.2.1.0 $\to$ L2
- 123.2.1.151 & 255.255.255.128 = 123.2.1.128 $\to$ L5
- 67.4.39.5 & 255.255.255.128 = 67.4.39.0 $\to$ L4

|network|netmask|interfaccia|next-hop|
|:-:|:-:|:-:|:-:|
|192.168.1.0|255.255.255.192|eth0|dir.conn.|
|123.2.1.0|255.255.255.128|eth0|192.168.1.2|

### tabella di instradamento dell’es

|network|netmask|interfaccia|next-hop|
|:-:|:-:|:-:|:-:|
|192.168.1.0|255.255.255.192|eth0|dir.conn.|
|127.0.0.0|255.0.0.0|Self/Io|dir.conn.|
|123.2.1.0|255.255.255.128|eth0|192.168.1.2|

### indirizzi IPv4 convenzionali

- 127.0.0.1 $\to$ se stessi
- <net>1…1 $\to$ broadcast nella net <net>
- 10.0.0.0/8 $\to$ indirizzi privati (non validi in Internet)

### distribuizione degli indirizzi IPv4

- IANA (Internet Assigned Numbers Authority)
- ARIN (American Registry for Internet Numbers)
- RIPE (Resèaux IP Europèen) Coordination Center
- APNIC (Asia Pacific Network Information Center)
- AfriNIC (Africa NIC)
- LACNIC (Latin America and Caribbean NIC)

### end system con più schede
gli es tendono ad avere varie schede

## ARP (Address Resolution Protocol)
un protocollo per scoprire quale indirizzo MAC corrisponde a un certo indirizzo IPv4
- RFC 826
- il protocollo ARP risolve il problema della traduzione di un indirizzo IPv4 in indirizzo MAC

> l’host invia un messaggio broadcast a tutti gli host di una rete
>
> l’host che ha quell’indirizzo IPv4 risponde con ARP reply, fornendo il proprio MAC

> mandare messaggi broadcast è costoso
> - gli host che usano ARP hanno a disposizione una cache in cui memorizzano le corrispondenze IPv4-MAC di cui sono venuti a conoscenza

```
arp -a
ping
```

### ARP – formato del pacchetto
- i campi hardware e protocol specificano il tipo di indirizzo di livello 2 e il tipo di indirizzo di livello 3
- i campi hlen e plen specificano la lunghezza dei due indirizzi (hardware e protocol)
- nel campo operation si specifica se il messaggio è una ARP request o una ARP reply
- i campi sender ia e ha sono rispettivamente l’indirizzo di livello 3 e di livello 2 del mittente
- il campo target ia contiene l’indirizzo di livello 3 della macchina di cui si vuole conoscere l’indirizzo di livello 2 nel caso di un ARP request
