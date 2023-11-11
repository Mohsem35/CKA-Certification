Agendas

1. [Switch](#switch)
2. [Router](#router)
3. [Default Gateway](#default-gateway)
4. [Troubleshooting Network](#troubleshooting-network)

### Switch

Switch **creats a network** and switching helps to **connect the interface** within same network

![switch](https://github.com/Mohsem35/CKA-Certification/assets/58659448/6aa1f387-12d6-4639-a2c4-f0e2bc277d53)

- To see the **interfaces on the hosts** use ip link command
```shell
ip link
```

To **connect to the switch** we use **`ip addr add`** command. 

- host machine এর interface তে ip address set করতে চাইলে **`ip addr add`** command use করতে হবে 

```shell
# ip addr add <host machine ip address>/24 dev <interface name>
ip addr add 192.168.1.10/24 dev eth0

# restart the system
```

For persist the change, set them in **`/etc/network/interfaces`**


### Router 

Router helps to connect to **two seprate networks together** and think of it as another server with many network port. Since it connects to two seperate networks, it **gets two IPs assigned**, one on each network. In first network we assign an IP address **`1.1`** and secind is **`2.1`**

**`192.168.1.1`** =  left side network এর gateway ip address

**`192.168.2.1`** = right side network এর gateway ip address


- To see the existing kernel's **routing table** configuration run the route command.
```shell
route -n
```


```shell
# output
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         172.16.4.1      0.0.0.0         UG    0      0        0 ens18
172.16.4.0      0.0.0.0         255.255.252.0   U     0      0        0 ens18
```

_destination = internet (0.0.0.0)_ তে যাইতে হলে, আমার _172.16.4.1 gateway_ use করতে হবে। 

আবার, _destination =  172.16.4.0_  তে যাইতে হলে _কোন gateway লাগবে না_, কারণ same network এর under  এ আছে 



To configure a **gateway** on system B to reach the system on other network run

- তাহলে system B এর routing table update করতে হবে, যাতে করে syetem C reach করা যায়

![routing](https://github.com/Mohsem35/CKA-Certification/assets/58659448/a609eb54-6858-4a9b-9a4d-fbeb9f73b9c9)


```shell

# Add entries to the routing table of system B

# ip route add <other network switching ip>/24 via <own network gateway ip>
ip route add 192.168.2.0/24 via 192.168.1.1
```

###  Default Gateway

- Any request to **any network** outside of your existing network

<img width="1364" alt="Screenshot 2023-11-11 at 12 10 46 PM" src="https://github.com/Mohsem35/CKA-Certification/assets/58659448/971d9186-bd7a-42ec-b039-d25ac12ebfa4">

```shell
ip route add default via <own network gateway ip>

or  

ip route add 0.0.0.0 via <own network gateway ip>
``` 


### Troubleshooting Network


**From Cline side**

_1. Check if the primary interface is up_

```shell
ip link
```

_2. Check if the hostname is resolved to ip addresses. Run **`nslookup`** command against the host name. **`nslookup`** command reaches out to the DNS server._

```shell
nslook up caleston-repo-01
```

_3. Ping remote server for response_

_4. Run the **`traceroute`** command. This will show us the number of hops or devices between the source and repo server_

**From Server side**

_5. Check is port is runnig_
```shell
netstat -an | grep 80 | grep -i LISTEN
```
_6. Check if the interface is UP_

```shell
ip link
```
UP command

```shell
ip link set dev <server interface name> up
```


#### Questions

_1. Inspect the interface `eth0` on devapp01, is it `UP`?_

```shell
ip link
``` 
look at the **status** of the interface starting with `eth0`

_2. Bring up the `eth0` interface_

```shell
sudo ip link set dev eth0 up
```

_3. While we are at it, there is also a missing `default route` on the server `devapp01.` Add the default route via `eth0` gateway._

To add the default route via eth0 gateway, run the following command: 

_1st: Find out the dafault fateway ip_

```shell
route -n

# output
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
172.16.238.0    0.0.0.0         255.255.255.0   U     0      0        0 eth0
172.16.239.0    0.0.0.0         255.255.255.0   U     0      0        0 eth1
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 eth2
```
_2nd: Add route_

```
sudo ip r add default via 172.16.238.1
```

_Extras:_

To delete the default route using the ip r command, you can use the following command:

```shell
sudo ip r del default
```
This command **removes the default route from the routing table**. Make sure to run the command with appropriate permissions, such as using sudo, to have the necessary privileges to modify the routing table.