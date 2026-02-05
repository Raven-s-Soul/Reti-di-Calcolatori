# Guide on Kathara

>- `lab.conf` descrive il network 
>- `subdirectories` configurazioni dei diversi dispositivi
>- `<device_name>.startup` azioni che esegue il dispositivo all avvio

<details>
<summary><h3>lab.conf</h3></summary><br>

> machine[arg]=value
> - machine = pc1 ...
>   - name from a-z
> - arg = eth port (number)
>   - from 0 to 9
> - value = address
>   - full adreess or only domani 

</details>

***

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

>kathara vstart -n pc1 --eth 0:A
>
>ip link set dev eth0 address 00:00:00:00:00:01


>kathara lconfig –n wireshark --add A

>p=Ether(dst='00:00:00:00:00:0B', src='00:00:00:00:00:0A')
>
>sendp(p, iface='eth0')

[^1]: https://www.kathara.org/man-pages/kathara.1.html
