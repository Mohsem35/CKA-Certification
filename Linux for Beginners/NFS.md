
Different types of storage architectures used in computing environments, each with its own characteristics and purposes.

**`DAS`** - Direct Attached Storage, **external storage is attached directly to the host** system tha requires the space.

**`NAS`** - **Network Attached Storage** quite similar to NFS server.

**`SAN`** - **Storage Aread Network**, this technology uses a **fiber channel** for providing high-speed storage. SAN is a **dedicated high-speed network** that connects and presents **shared pools** of storage devices to **multiple servers**

#### NFS


NFS stands for **Network File System**. It is a distributed **file system protocol** that allows a user on a **client computer to access files over a network** as if they were stored locally on the client's own hard drive. NFS **enables file sharing between computers in a network**, providing a straightforward way to access and manage files across different systems.

The term for a directory sharing and NFS is called **`exporting`**

- NFS - Does not store data in blocks. Instead, it **saves data in form of files**. It works on service-client model.
- **NFS server maintains** an export configuration file at **`/etc/exports`** that defines the clients which should be able to access the directories on the server. `/etc/exports` **looks like** this

```shell 
/etc/exports

# configuration at NFS side
# /software/repos <host_name_of_the_clients>
/software/repos 10.61.35.201 10.61.35.202 10.61.35.203
```

- To **exports all the mounts** defined in `/etc/exports` use
```shell
# NFS side
exportfs -a
```
- To **manually export a directory** in NFS server, run the following command
```shell
# NFS side
exportfs -o 10.61.35.201:/software/repos
```

NFS side থেকে একবার export হয়ে গেলে, এখন clinet side থেকে mount configure করতে হবে 

```shell
# Client side
# mount <NFS_server_IP>:/software/repos /mnt/software/repos
mount 10.61.112.101:/software/repos /mnt/software/repos
```