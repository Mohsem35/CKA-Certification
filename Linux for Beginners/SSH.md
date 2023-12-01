**Password-Less Authentication**

**Passwordless authentication** can be setup via **key-pair authentication** in order to login to the remote server with password.

**key pair = private key + public key**
    
- To **generate a keypair** on the Client run this command

```shell
ssh-keygen –t rsa
```

**`rsa`** = cryptographic algorithm which will encrypt key

**`id_rsa`** = private key will **not be shared** with server

**`id_rsa.pub`** = public key will be shared with server

- To **copy the Public key** from the client to the remote server


```shell
ssh-copy-id username@ipaddress
```
- public key টাকে আমি remote server তে সেন্ড করে দিব, এবং remote server সেইটা save করে রাখবে 

```shell
cat /home/bob/.ssh/authorized_keys
```

#### COPYING DIRECTORIES

- To copy a **directory to a remote server**
```shell
scp –pr /home/bob/media/ devapp01:/home/bob
```

_Q1: Which port number does the SSH service use by default?_

port 22
