Agendas

1. [Switch](#switch)
2. [Router](#router)


### Switch

Switch **creats a network** and switching helps to **connect the interface** within same network


- To see the **interfaces on the hosts** use ip link command
```shell
ip link
```

- To **connect to the switch** we use **`ip addr add`** command. 

**`ip addr add`** is used to **set ip addresses on the interfaces** 
```shell
# ip addr add <host machine ip address>/24 dev <interface name>
ip addr add 192.168.1.10/24 dev eth0

# restart the system
```

For persist the change, set them in **`/etc/network/interfaces`**


### Router 

Router helps to connect to **two seprate networks together** and think of it as another server with many network port. Since it connects to two seperate networks, it **gets two IPs assigned**, one on each network. In first network we assign an IP address **`1.1`** and secind is **`2.1`**



- To see the existing kernel's **routing table** configuration run the route command.
```shell
route -n
```

_destination = internet (0.0.0.0)_ তে যাইতে হলে, আমার _172.16.4.1 gateway_ use করতে হবে। 

আবার, _destination =  172.16.4.0_  তে যাইতে হলে _কোন gateway লাগবে না_, কারণ same network এর under  এ আছে 


```shell
# output
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         172.16.4.1      0.0.0.0         UG    0      0        0 ens18
172.16.4.0      0.0.0.0         255.255.252.0   U     0      0        0 ens18
```


To configure a **gateway** on system B to reach the system on other network run

- তাহলে system B এর routing table update করতে হবে, যাতে করে syetem C reach করা যায় 

```shell

# Add entries to the routing table of system B

# ip route add <other network switching ip>/24 via <own network gateway ip>
ip route add 192.168.2.0/24 via 192.168.1.1
```

- Any request to **any network** outside of your existing network 
```shell
ip route add default via <own network gateway ip>

or  

ip route add 0.0.0.0 via <own network gateway ip>
``` 