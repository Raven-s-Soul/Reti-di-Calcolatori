# Esercizio: posta elettronica e DNS

## La Velocità della Rete: Store & Forward (S&F)
Nelle reti a commutazione di pacchetto di tipo **Store & Forward**, ogni nodo (switch o router) deve ricevere l'intero pacchetto prima di poterlo inoltrare sul link successivo.

### 1. Parametri Fondamentali
* **$b$**: Dimensione totale del pacchetto espressa in bit.
* **$B$**: Larghezza di banda (velocità) del collegamento in bit/sec.
* **$t_l = b/B$**: Tempo necessario per trasmettere il pacchetto su un singolo collegamento.

### 2. Calcolo del Tempo Totale ($T$)
Il tempo totale $T$ (dal primo bit spedito all'ultimo bit ricevuto) si calcola moltiplicando il tempo di trasmissione del singolo link ($t_l$) per il numero di collegamenti ($N$) attraversati:
$$T = N \times \frac{b}{B}$$
