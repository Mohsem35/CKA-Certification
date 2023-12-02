Network security firewall

- Cisco ASA
- Juniper NG firewall
- Barracuda NG firewall
- Fortinet 

### IPTABLES

Iptables uses a set of tables that have **chains** that contain a set of **built-in or user-defined rules**.

The two types of tables/rules:

1. FILTER – this is the default table, which **contains the built-in chains** for  
    - **`INPUT`** – packages destined for local sockets.
    - **`FORWARD`** – packets routed \through the system. 
    - **`OUTPUT`** – packets generated locally.
2. NAT – a table that is consulted when **a packet tries to create a new connection**. It has the following built-in: 
    - **`PREROUTING`** – used for altering a packet as soon as it’s received
    - **`OUTPUT`** – used for altering locally-generated packets. 
    - **`POSTROUTING`** – used for altering packets as they are about to go out.



For **installing** IPtables in Ubuntu servers,
```shell
sudo apt install iptables
```

- To **list** the iptables rules,
```shell
sudo iptables -L

# output
Chain INPUT (policy ACCEPT)
target     prot opt source               destination

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```

#### INCOMING TRAFFIC

- To **allow incoming connection** from IP **172.16.238.187** to port `22` and `80`, you can run the following command.
```shell
sudo iptables -A INPUT -p TCP -s 172.16.238.187 --dport 22 -j ACCEPT
```
```shell
sudo iptables -A INPUT -p TCP -s 172.16.238.187 --dport 80 -j ACCEPT
```

- To **allow incoming TCP traffic** on ports 22 (SSH), 80 (HTTP), and 443 (HTTPS)
```shell
iptables -A INPUT  -p tcp -m multiport --dports 22,80,443 -j ACCEPT
```

```
-A INPUT: Appends the rule to the INPUT chain
-p TCP: Specifies the protocol as TCP
-s 172.16.238.187: Specifies the source IP address 172.16.238.187
--dport 22: Specifies the destination port 22
```

`-A` or --append option appends the rule at the **end of the selected chain**

- To **drop any incoming connections** from **any source on any destination port** for any protocol


```shell
sudo iptables -A INPUT -j DROP
```


```shell
sudo iptables -L

# output
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
ACCEPT     tcp  --  caleston-lp10        anywhere             tcp dpt:ssh
ACCEPT     tcp  --  caleston-lp10        anywhere             tcp dpt:ssh
ACCEPT     tcp  --  caleston-lp10        anywhere             tcp dpt:http
DROP       all  --  anywhere             anywhere

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```



`DROP` and `REJECT` দুইটাই **prohibits packets** from passing **through the firewall**. কিন্তু  the main difference between them is the **response message**


DROP কোন রকমের indication message client or server কে sent করে না, কিন্তু REJECT **sends an error message** back to the source indicating a connection failure

#### OUTGOING TRAFFIC

- To **block outgoing traffic** to any destination **on port 80**
```shell
sudo iptables -A OUTPUT -p tcp --dport 80 -j DROP
```
```shell
sudo iptables -L

# output
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
ACCEPT     tcp  --  caleston-lp10        anywhere             tcp dpt:ssh
ACCEPT     tcp  --  caleston-lp10        anywhere             tcp dpt:ssh
ACCEPT     tcp  --  caleston-lp10        anywhere             tcp dpt:http
DROP       all  --  anywhere             anywhere

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
DROP       tcp  --  anywhere             anywhere             tcp dpt:http
```

- To **allow outgoing TCP traffic** with source ports `22` (SSH), `80` (HTTP), and `443` (HTTPS)

```shell
iptables -A OUTPUT -p tcp -m multiport --sports 22,80,443 -j ACCEPT
```

- To **allow outgoing TCP traffic** from your system to the host google.com on port 443 (HTTPS)
```shell
sudo iptables -I OUTPUT -p tcp -d google.com --dport 443 -j ACCEPT
```

#### DELETE RULES

```shell
# First find the line-number of the rule using the command below
sudo iptables -L --line-numbers

Chain INPUT (policy ACCEPT)
num  target     prot opt source               destination
1    ACCEPT     tcp  --  caleston-lp10        anywhere             tcp dpt:ssh
2    ACCEPT     tcp  --  caleston-lp10        anywhere             tcp dpt:ssh
3    DROP       all  --  anywhere             anywhere

Chain FORWARD (policy ACCEPT)
num  target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
num  target     prot opt source               destination
1    ACCEPT     tcp  --  anywhere             google.com           tcp dpt:https
2    ACCEPT     tcp  --  anywhere             devdb01              tcp dpt:postgresql
3    ACCEPT     tcp  --  anywhere             caleston-repo-01     tcp dpt:http
4    DROP       tcp  --  anywhere             anywhere             tcp dpt:http
5    DROP       tcp  --  anywhere             anywhere             tcp dpt:https
```
```shell
# Now if you want to delete the INPUT rule number 3, run
sudo iptables -D INPUT 3
```


> `-I` inserts the rule to the top of the chain instead of the buttom. This will add the accept rule to the first position in the chain. মানে rules টা সবার উপড়ে add হবে `I` দিয়ে commmand execute করলে, normally `A` দিয়ে append করলে rules গুলো সবার শেষে যুক্ত হয়  

#### OTHERS


- To **Block Incoming Ping Requests** on IPtables on an interface say **eth0**,

```shell
iptables -A INPUT -p icmp -i eth0 -j DROP
```

- To **Block Access** to Specific **MAC Address** on IPtables
```shell
iptables -A INPUT -m mac --mac-source 0e:Ds:8n:mq:00:de -j DROP

0e:Ds:8n:mq:00:de refers to mac address to be blocked
```

#### BEST PRACTICES

Database সার্ভারে যাতে শুধুমাত্র api services গুলো access করতে পারে, সেজন্য DB সার্ভারে ip tables rule add করতে হবে 

```shell
iptables -A INPUT -p tcp -s <api_server_ip> --dport 5432 -j ACCEPT
```

next rule হছে, DB server সব রকমের outbound traffic DROP করবে 
```shell
iptables -A INPUT -p tcp --dport 5432 -j DROP
```

#### CRONJOB

```shell
crontab -e
```

```shell
crontab -l
```

_Q: The script /usr/local/bin/system-debugger.sh was incorrectly scheduled. It should run every half hour at minute 0 and minute 30_

Example: 09:00, 09:30, 10:00, 10:30, 11:00, 11:30……so on every half hour.



```shell
# use Step values for the minute column = */30 OR

*/30 * * * * /usr/local/bin/system-debugger.sh
```