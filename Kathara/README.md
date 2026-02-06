# Guide on Kathara

### File structure

>- `lab.conf` descrive il network 
>- `subdirectories` configurazioni dei diversi dispositivi
>- `<device_name>.startup` azioni che esegue il dispositivo all avvio

<details>
<summary><h4>lab.conf</h4></summary><br>

> machine[arg]=value
> - machine = pc1 ...
>   - name from a-z
> - arg = eth port (number)
>   - from 0 to 9
> - value = address
>   - full adreess or only domani 

</details>

***
## Example Tools

>kathara vstart -n pc1 --eth 0:A
>
>ip link set dev eth0 address 00:00:00:00:00:01

<details>
<summary><h3>scapy - python</h3></summary><br>

```python
p=Ether(dst='00:00:00:00:00:0B', src='00:00:00:00:00:0A')
sendp(p, iface='eth0')
```
</details>

<details>
<summary><h3>wireshark</h3></summary><br>

lab.conf
```console 
wireshark[bridged]=true
wireshark[port]="3000:3000"
wireshark[image]="lscr.io/linuxserver/wireshark"
wireshark[num_terms]=0
```

```bash
kathara lconfig –n wireshark --add A
```
</details>

<details>
<summary><h3>bridge</h3></summary><br>

lab.conf
```console 
X[0]="A/00:00:00:00:00:b1"
X[1]="B/00:00:00:00:00:b2"
X[2]="C/00:00:00:00:00:b3"
X[3]="D/00:00:00:00:00:b4"
X[image]="kathara/base"
X[ipv6]="false"
```

X.startup
```bash
ip link add name <mainbridge name> type bridge
ip link set dev ethX master <mainbridge name>
ip link set up dev <mainbridge name>  # default down
brctl setageing <mainbridge name> 600 # default 300
```

</details>

***
## Commands

|prefix|meaning|
| :--: | :-- |
|v| low level - single target|
|l| high level - multy target|
||Global - Mangagment commadnds|

|base commands[^1]|meaning @ ={ v, l }|
| :--: | :-- |
|@start| Start a new Kathara (v)device (l)network scenario |
|@clean| Stop a (v)single Kathara device (l)network scenario |
|@config| Manage the network interfaces of a running Kathara device (l)in a network |
|lrestart| Restart a Kathara network scenario |
|linfo| Show information about a Kathara network scenario |
|connect|Connect to a Kathara device |
|exec|Execute a command in a Kathara device |
|wipe| Delete all Kathara devices and collision domains, optionally also delete settings |
|list| Show all running Kathara devices |
|settings| Show and edit settings |
|check| Check your system environment |


[^1]: https://www.kathara.org/man-pages/kathara.1.html
