# Subnetting IPv4

| CIDR | Bit Host ($h=32-n$) | Hosts Totali ($2^h$) | Host Usabili ($2^h - 2$) | Subnet Mask (256-usabili) |
| :--- | :---: | :---: | :---: | :--- |
| **/32** | 0 | 1 | 1 (Host singolo) | 255.255.255.255 |
| **/31** | 1 | 2 | 0 (o 2 su P2P) | 255.255.255.254 |
|||||||
| **/30** | 2 | 4 | 2 | 255.255.255.252 |
| **/29** | 3 | 8 | 6 | 255.255.255.248 |
| **/28** | 4 | 16 | 14 | 255.255.255.240 |
| **/27** | 5 | 32 | 30 | 255.255.255.224 |
| **/26** | 6 | 64 | 62 | 255.255.255.192 |
| **/25** | 7 | 128 | 126 | 255.255.255.128 |
| **/24** | 8 | 256 | 254 | 255.255.255.0 |
| **/23** | 9 | 512 | 510 | 255.255.254.0 |
| **/22** | 10 | 1.024 | 1.022 | 255.255.252.0 |
| **/21** | 11 | 2.048 | 2.046 | 255.255.248.0 |
| **/20** | 12 | 4.096 | 4.094 | 255.255.240.0 |
| **/19** | 13 | 8.192 | 8.190 | 255.255.224.0 |
| **/18** | 14 | 16.384 | 16.382 | 255.255.192.0 |
| **/17** | 15 | 32.768 | 32.766 | 255.255.128.0 |
| **/16** | 16 | 65.536 | 65.534 | 255.255.0.0 |
| **/8** | 24 | 16.777.216 | 16.777.214 | 255.0.0.0 |

Alternativa a questo è scrivere i bit e contare fino dividere da pre | a dopo.
- trovando una sezioni dove la subnet è gestita da i bit pre |

>[!NOTE]
> il il x.x.x.0 (primo indirizzo) e l'ultimo indirizzo x.x.x.y sono riservati respettivamente per la net e per il broadcast

## Come si trova la giusta rete
`12.34.56.78/28` per esempio, seguiamo il processo
- $32-28 = 4 \to 2^4 = 16$
- 16 - 32 - 48 - 64 - 80 - ...
- è tra 64 e 80, percui appartiene al `12.34.56.64/28` con netmask `255.255.255.240` e broadcast 80-1 $\to$ `12.34.56.79/28`
