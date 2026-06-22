# Il modello iso-osi

>### Gli strati del modello
>- applicazione
>- presentazione
>- sessione
>- trasporto
>- rete
>- data link
>- fisico
>
>>- ogni strato corrisponde ad un livello di astrazione
>>- ogni strato offre funzioni ben definite
>>- lo scambio di dati tra livelli è minimizzato
>>- scelta del numero di livelli
>>   - funzioni distinte su livelli diversi
>>   - fattibilità

## Concetti Fondamentali:
- Strato/Livello
- Protocollo
- Interfaccia

***

> all’interno di uno strato si possono individuare una o più entità
>
> il punto logico in cui uno strato offre il servizio allo strato superiore è detto service access point (sap)
>
> indipendenza funzionale:
> - ogni strato è definito in modo del tutto indipendente dallo strato sottostante, l'unico punto di contatto è l’interfaccia

>i dati generati da un protocollo di livello n sono detti n-pdu = n-protocol data unit
>- pdu = header + payload


## strato fisico (1)
>si interfaccia direttamente con il mezzo trasmissivo

## strato data-link (2)
>obiettivo:
>
>- fronteggiare eventuali malfunzionamenti dello strato fisico (rilevazione e correzione degli errori)
>
> servizi offerti allo strato di rete:
>- trasferimento di pdu tra sistemi adiacenti
>
> utilizzo di due code, nelle due direzioni

## strato di rete (3)

>conosce la topologia (la mappa) della rete
>- instradamento
>
>il servizio offerto dallo strato di rete consente di:
>- trasferimento di pdu da estremo ad estremo
>  
>commutazione:
>- circuito, pacchetto datagramma, pacchetto circuito virtuale

### servizi e protocolli connessi e non

> per tutti i livelli superiori al primo sono pensabili due modalità operative, che danno luogo a due tipi di servizi e di protocolli: connessi o non connessi

|Tipo|Affidabilità|Efficenza|Esempio|
|:-:|:-:|:-:|:-:|
|Non Connesso|-|+|Email|
|Connesso|+|-|Telefono|

## strato di trasporto (4)

- colma deficienze e fluttuazioni della qualità del servizio dello strato di rete
- è il primo strato estremo-estremo (risiede solo nei nodi terminali)
- servizi offerti allo strato 5:
   - instaurazione di una connessione
   - trasferimento affidabile dei dati e gestione della connessione
   - rilascio della connessione


|Livello|LAN|WAN|
|:-:|:-:|:-:|
|2|No|No|
|3|No|No|
|4|Si|Si|

## strati vicini alle applicazioni (5, 6 e 7)
- sessione: sincronizzazione del dialogo tra due processi
- presentazione: scambio di messaggi indipendente dalla sintassi della trasmissione
- applicazione: mezzo per accedere alla rete per un processo applicativo

> i tre strati sono spesso più in parallelo che in serie

> una connessione di livello n+1 che sfrutta più connessioni di livello n

> IEEE 802 livello 1 e 2


























