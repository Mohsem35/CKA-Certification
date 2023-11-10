### DNS



The domain name system is a distributed way to share **name-to-IP** associations _instead of requiring each computer to synchronize a hosts file_. A name server publishes the IP address for a domain and provides a single location to update when an IP changes

#### Ping

**`Ping`** Command is use to check the remote machine is **reachable or not**

```shell
ping 192.168.1.11
```

> _Note:_ নিজের pc থেকে অথবা host-machine থেকে domain name resolve করতে চাইলে নিচের procedure follow করতে হবে 

- To Ping the remote host with a **name instead of IP Address** make an entry in **`/etc/hosts`** file


```shell
# edit the hosts file
sudo vim /etc/hosts
192.168.1.11 db

# now ping from terminal
ping db
```

> _Note:_ You can configure as many host you want in the /etc/hosts file.


#### DNS Resolution




**`/etc/resolv.conf`** file contains **information about the DNS servers** that the system should use to resolve domain names to IP addresses. Typically, it includes the IP addresses of DNS servers that the system should **query** when trying to resolve domain names


- Every host has a DNS resolution file **`/etc/resolv.conf`**

```shell
cat /etc/resolv.conf
nameserver 192.168.1.100    
```

এখানে DNS এর server ip হল 192.168.1.100

> _Note:_ Host machine domain name resolve করার জন্য **`/etc/resolv.conf`** ফাইলে গিয়ে **`nameserver`** options টা খুঁজতে থাকবে 


- The **`/etc/nsswitch.conf`** file is used to configure which services are to be used to determine information **preference** such as hostnames, password files, and group files.There is a specific search order according to which it is performed. This order is set in this configuration file.


```
cat /etc/nsswitch.conf
```
In  here, **1st preference is /etc/hosts file** and 2nd preference is dns server

```shell
# output
hosts:          files dns
networks:       files
```

**বাহিরের দুনিয়ার কোন server(facebook, chatgpt) use করতে চাইলে**, DNS server এর configuration ফাইলের নিচে **`Forward All to 8.8.8.8`** লাইন add করলেই হবে 


### Domain Names 



### Record Types

maps.google.com is a subdomain of googl.com 


In the context of domain names and the Domain Name System (DNS), various types of DNS records exist to serve different purposes. DNS records are used to translate human-readable domain names into IP addresses and provide other information related to domain configuration. Here are some common DNS record types:


- **`A (Address)`** Record: Associates a _domain name with an IPv4 address_.

- **`AAAA (IPv6 Address)`** Record: Similar to the A record but associates a _domain name with an IPv6 address_.
- **`CNAME (Canonical Name)`** Record: Creates an _alias or nickname for a domain_. It is often used to point one domain to another.




To test the DNS resolution you can use **`nslookup`**, **`traceroute`**, **`dig`** command, this will query a hostname from a DNS Server.

```shell
nslookup www.google.com
traceroute www.google.com
dig www.google.com
```