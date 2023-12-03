
### List all Block devices

- **Block devices** are special files that refer to or represent a device (which could be **anything** from a **hard drive to a USB drive**). So naturally, there are command line tools that help you with your block devices-related work.

- **Major Number** is used to identigy the **type of block device**, value 8 represent a SCSI device starts with SD.

- **Minor Number** is uset to distuinguish individual, physical or logical devices.
```shell
lsblk 

ls -l /dev/ | grep "^b"
```

- To Print, Create and Delete the parition table use `fdisk -l` command
```shell
sudo fdisk -l /dev/sda

# output
Disk /dev/sda: 100 GiB, 107374182400 bytes, 209715200 sectors
Disk model: QEMU HARDDISK   
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: C9E7CB2E-D91A-4D9C-A8A8-4CFF36E65F77

Device     Start       End   Sectors  Size Type
/dev/sda1   2048      4095      2048    1M BIOS boot
/dev/sda2   4096 209715166 209711071  100G Linux filesystem
```

**Partition Types**

1. **`PRIMARY`** - Use to **Boot an Operating System**.
2. **`EXTENDED`** - Can host logical partitions but cannot be used on its own.
3. **`LOGICAL`** - Created within an extended partition


#### Creating Partitions

**`Gdisk`** is an **improved version** of the fdisk that works with the **GTP partition table**

**`MBR`** (Master Boot Record)

**`GPT`** (GUID Partition Table)

To create a partition on **`sdb`** use
```shell
gdisk /dev/sdb
GPT fdisk (gdisk) version 1.0.1

Partition table scan:
    MBR: protective
    BSD: not present
    APM: not present
    GPT: present
Found valid GPT with protective MBR; using GPT.

Command (? for help): ?
b back up GPT data to a file
c change a partition's name
d delete a partition
i show detailed information on a partition
l list known partition types
n add a new partition
o create a new empty GUID partition table (GPT)
p print the partition table
q quit without saving changes
r recovery and transformation options (experts only)
s sort partitions
t change a partition's type code
v verify disk
w write table to disk and exit
x extra functionality (experts only)
? print this menu

Command (? for help): n
Partition number (1-128, default 1): 1
First sector (34-41943006, default = 2048) or {+-}size{KMGTP}: 2048
Information: Moved requested sector from 34 to 2048 in
order to align on 2048-sector boundaries.
Use 'l' on the experts' menu to adjust alignment
Last sector (2048-41943006, default = 41943006) or {+-}size{KMGTP}: 41943006
Current type is 'Linux filesystem'
Hex code or GUID (L to show codes, Enter = 8300):
Changed type of partition to 'Linux filesystem'
Command (? for help): w
Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
PARTITIONS!!
Do you want to proceed? (Y/N): Y
OK; writing new GUID partition table (GPT) to /dev/vdb.
The operation has completed successfully.
```

##### Questions





### File Systems in Linux

#### Working with Ext4 (most important)


1. To **create a file system with Ext4**, we will make use of **`/dev/sdb`** disk
```shell
mkfs.ext4 /dev/sdb1
```
2. Now **create a directory to mount the filesystem** 

```shell
mkdir /mnt/ext4
mount /dev/sdb1 /mnt/ext4
```
3. To **verify** if the filesystem is mounted use

```shell
mount | grep /dev/sdb1
df -hP | grep /dev/sdb1
```
4. Add an entry into **`/etc/fstab`** for the filesystem to be available after reboot


```shell
echo "/dev/sdb1 /mnt/ext4 ext4 rw 0 0" >> /etc/fstab
```


**`fstab`** file attributes

| FIELD  | Purpose |
| ------------- | ------------- |
| Filesystem  | Such as /dev/vdb1 to be mounted  |
| Mountpoint  | Directory to be mounted on  |
| Type | Example ext2, ext3, ext4  |
| Options  | Such as RW = Read-write, RO = Read Only  |
| Dump | 0 = Ignore, 1 = take backup  |
| Pass  | 0= ignore, 1 or 2 - FSCK filesystem check enforced  |



#### Questions

##### GPT Partition

```
bob@caleston-lp10:~$ lsblk
NAME                   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
vda                    252:0    0   10G  0 disk 
└─vda1                 252:1    0   10G  0 part 
  ├─vagrant--vg-root   253:0    0    9G  0 lvm  /
  └─vagrant--vg-swap_1 253:1    0  980M  0 lvm  [SWAP]
vdb                    252:16   0    1G  0 disk 
vdc                    252:32   0    1G  0 disk 
```
_Q1: How many disk type **block devices are present** in the system?_

3

_Q2: What is the **major number** for the devices beginning with vd?_

```
ls -l /dev/vd*
```
252

_Q3: How many **maximum partitions** (primary or extended ) can an **MBR have**?_

4


_Q4: **Create a GPT partition** called **`vdb1`** of size `500M` on the disk **`/dev/vdb`**_

_You can install `gdisk` by running `sudo apt install gdisk`_


```
sudo gdisk /dev/vdb
```

In the interactive prompt, enter `n`

Select parition number = `1` (for vdd1)

Select default first sector = `2048`

Select `+500M` when asked for last sector

Use default hex code = `8300`

Finally type `w` to write to the partition table and `Y`

##### Mount Filesystem


_Q1: Which of the following filesystems **does not use a journal**?_

`EXT2` does not use a journal and hence has longer recovery boot time.

_Q2: Out of the disks `/dev/vdb` and `/dev/vdc`, which one has a **filesystem created**?_

```shell
sudo df -h
```
এই command দিয়ে চেক করে দেখব, filesystem করা আছে কিনা। filesyatem করা না থাকলে mount করা possible না 

`/dev/vdc` is mounted at `/mnt/backups`. This is only **possible if it has a filesystem**


_Q3: Can you **find out the type of filesystem** created in `/dev/vdc`?_

> Use the `blkid` command with the disk name as the argument.
```shell
sudo blkid /dev/vdc

# output
/dev/vdc: UUID="c56b9985-2c64-40d3-8191-80a1f3f12729" TYPE="ext2"
``` 
type ext2

_Q4: Create an `ext4` filesystem on the disk `/dev/vdb` and ***mount*** it at `/mnt/data`_

```shell
sudo mkfs.ext4 /dev/vdb

sudo mkdir /mnt/data

sudo  mount /dev/vdb /mnt/data
```

_Q5: What would happen to the mount `/mnt/data` after a system reboot?_


_Q6: Make the **mount persistent across reboot**_

1st step: Use `rw` option with the `dump` and `pass` numbers both set to `0`

2nd step: Add it in the FSTAB
```
sudo vi /etc/fstab
```

3rd step: Add the line
```
/dev/vdb /mnt/data ext4 rw 0 0
```
Save and Exit


External storage

DAS, NAS and SAN

connected through fiber channel

data traffic between the host and storage is through the network









