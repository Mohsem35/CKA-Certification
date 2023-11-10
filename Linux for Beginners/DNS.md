
Agendas

1. [DNS](#dns)
2. [Domain Names](#domain-names)
3. [Record Types](#record-types)



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

<img width="800" alt="Screenshot 2023-11-10 at 11 03 22 AM" src="https://github.com/Mohsem35/CKA-Certification/assets/58659448/6a7fd81e-9a55-447c-b00c-8d2c4fa4b837">

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

<img width="800" alt="Screenshot 2023-11-10 at 11 17 37 AM" src="https://github.com/Mohsem35/CKA-Certification/assets/58659448/050abd80-7bb9-4683-837e-7168a8f14e4a">

**বাহিরের দুনিয়ার কোন server(facebook, chatgpt) use করতে চাইলে**, DNS server এর configuration ফাইলের নিচে **`Forward All to 8.8.8.8`** লাইন add করলেই হবে 


### Domain Names 

![root](https://github.com/Mohsem35/CKA-Certification/assets/58659448/ece1aabb-e23d-40b0-9e2f-9dd92d554167)

`maps.google.com` is a **subdomain** of `googl.com` 

### Record Types

In the context of domain names and the Domain Name System (DNS), various types of DNS records exist to serve different purposes. DNS records are used to translate human-readable domain names into IP addresses and provide other information related to domain configuration. Here are some common DNS record types:


- **`A (Address)`** Record: Associates a _domain name with an IPv4 address_.

- **`AAAA (IPv6 Address)`** Record: Similar to the A record but associates a _domain name with an IPv6 address_.
- **`CNAME (Canonical Name)`** Record: Creates an _alias or nickname for a domain_. It is often used to point one domain to another.


- To test the DNS resolution you can use **`nslookup`**, **`traceroute`**, **`dig`** command, this will query a hostname from a DNS Server.

```shell
nslookup www.google.com
traceroute www.google.com
dig www.google.com
```



#### Questions

_1. What is the IP address of the DNS Server used in this system?_

`cat /etc/resolv.conf`

_2.Which file is responsible for host file-based DNS resolution?_

`/etc/hosts`

_3. Change the DNS Server to google's DNS which is 8.8.8.8_

change the nameserver IP to 8.8.8.8 at `/etc/resolv.conf`

_4. Which order is used currently to resolve an IP address in the system?_

Check `hosts` line of the `nsswitch.conf` file 