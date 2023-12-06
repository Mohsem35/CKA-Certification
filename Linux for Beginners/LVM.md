

LVM allows **grouping of multiple physical volumes**, which are hard disks or partitions into a volume group.



Working with LVM

1st step: To make use of LVM, **install the package LVM**

```shell
apt-get install lvm2
```

Check if physical volume exists
```shell
pvs
lvmdiskscan
```

2nd step: Use **`pvcreate`** command to create a **Physical Volume**

```shell
pvcreate /dev/sdb
Physical volume "/dev/sdb" successfully created
```

3rd step: Use **`vgcreate`** command to create a **Volume Group**

```shell
vgcreate caleston_vg /dev/sdb
Volume group "caleston_vg" successfully created
```

4th step: Use **`pvdisplay`** command to **list all the PVs** their names, size and the Volume group it is part of.

```shell
pvdisplay

--- Physical volume ---
  PV Name /dev/sdb
  VG Name caleston_vg
  PV Size 20.00 GiB / not usable 3.00 MiB
  Allocatable yes
  PE Size 4.00 MiB
  Total PE 5119
  Free PE 5119
  Allocated PE 0
  PV UUID iDCXIN-En2h-5ilJ-Yjqv-GcsR-gDfV-zaf66E

```

5th step: Use **`vgdisplay`** to see more **details of the VG**

```shwll
vgdisplay

--- Volume group ---
  VG Name caleston_vg
  System ID
  Format lvm2
  Metadata Areas 1
  Metadata Sequence No 1
  VG Access read/write
  VG Status resizable
  MAX LV 0
  Cur LV 0
  Open LV 0
  Max PV 0
  Cur PV 1
  Act PV 1
  VG Size 20.00 GiB
  PE Size 4.00 MiB
  Total PE 5119
  Alloc PE / Size 0 / 0
  Free PE / Size 5119 / 20.00 GiB
  VG UUID VzmIAn-9cEl5bA-lVtm-wHKX-KQaObR
```
6th step: To **create the Logical Volumes**, you can use **`lvcreate`** command

```shell
lvcreate –L 1G –n vol1 caleston_vg
Logical volume "vol1" created
```

7th step: To **display the Logical Volumes**, you can use lvdisplay command

```shell
lvdisplay

--- Logical volume ---
  LV Path /dev/caleston_vg/vol1
  LV Name vol1
  VG Name caleston_vg
  LV UUID LueYC3-VWpE31-UaYk-wjIR-FjAOyL
  LV Write Access read/write
  LV Creation host, time master, 2020-03-31 06:26:14
  LV Status available
  # open 0
  LV Size 1.00 GiB
  Current LE 256
  Segments 1
  Allocation inherit
  Read ahead sectors auto
  - currently set to 256
  Block device 252:0

```
To **list the volume**, you can use lvs command

```shell
lvs

 LV VG Attr LSize Pool
 vol1 caleston_vg -wi-a----- 1.00g
```


8th step: Now to **create an filesystem** you can use **`mkfs`** command

```shell
mkfs.ext4 /dev/caleston_vg/vol1
```

9th step: To mount the filesystem use mount command

```shell
mount –t ext4 /dev/caleston_vg/vol1 /mnt/vol1
```

10th step: Now logical volume is now available for use. Lets **resize the filesystem on vol1** while it is mounted. Check the free space available.

```shell
vgs
VG         #PV#LV#SN Attr VSize VFree
caleston_vg 1 1 0 wz--n- 20.00g 19.00g

lvresize -L +1G -n /dev/caleston_vg/vol1
Logical volume vol1 successfully resized.

df –hP /mnt/vol1
Filesystem Size Used Avail Use% Mounted on
/dev/mapper/caleston_vg-vol1 976M 1.3M 908M 1% /mnt/vol1
```


11th step: Now to **resize the file system** use **`resize2fs`** command

```shell
resize2fs /dev/caleston_vg/vol1

resize2fs 1.42.13 (17-May-2015)
Filesystem at /dev/mapper/caleston_vg-vol1 is mounted on
/mnt/vol1; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 1
The filesystem on //dev/mapper/caleston_vg-vol1 is now 524288
(4k) blocks long.
```

11th step: Now run **`df -hp`** command to **verify** the size of the mounted filesystem

```shell
df –hP /mnt/vol1

Filesystem Size Used Avail Use% Mounted on
/dev/mapper/caleston_vg-vol1 2.0G 1.6M 1.9G 1% /mnt/vol1
```



##### Questions

_Q1: Is LVM installed on this machine?_

Try out commands such as **`pvdisplay`** or **`lvs`**


```
LV     VG         Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
root   vagrant-vg -wi-ao----  <9.04g                                                    
swap_1 vagrant-vg -wi-ao---- 980.00m                                                    
```
yes

_Q2: LVM is installed on this machine and you will find that a physical volume has already been created._

_What is the name of this physical volume?_

use **`pvdisplay`** command


_Q3: Now, we will create a new VG. To do this we will make use of the disks `/dev/vdb` and `/dev/vdc`_

_What are the size of these disks individually?_

```shell
sudo lsblk
``` 
and inspect the size of disks. Each disk is of size 1G

_Q4: Create PV's using `/dev/vdb` and `/dev/vdc`_

```shell
sudo pvcreate /dev/vdb /dev/vdc
```

_Q5: Create a new `volume group` called `caleston_vg` using the newly created `PV's`_

```shell
sudo vgcreate caleston_vg /dev/vdb /dev/vdc
```

_Q6: Create a new logical volume called `data` from the `caleston_vg`._

_Size of the volume should be 1G_

```shell
sudo lvcreate -L 1G -n data caleston_vg
```
```
root@caleston-lp10:~# lsblk
NAME                   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
vda                    252:0    0   10G  0 disk 
└─vda1                 252:1    0   10G  0 part 
  ├─vagrant--vg-root   253:0    0    9G  0 lvm  /
  └─vagrant--vg-swap_1 253:1    0  980M  0 lvm  [SWAP]
vdb                    252:16   0    1G  0 disk 
└─caleston_vg-data     253:2    0    1G  0 lvm  
vdc                    252:32   0    1G  0 disk 
└─caleston_vg-data     253:2    0    1G  0 lvm  
```
```
root@caleston-lp10:~# df -hT
Filesystem                   Type      Size  Used Avail Use% Mounted on
udev                         devtmpfs  461M     0  461M   0% /dev
tmpfs                        tmpfs      99M  5.3M   94M   6% /run
/dev/mapper/vagrant--vg-root ext4      8.9G  1.5G  7.0G  18% /
tmpfs                        tmpfs     493M     0  493M   0% /dev/shm
tmpfs                        tmpfs     5.0M     0  5.0M   0% /run/lock
tmpfs                        tmpfs     493M     0  493M   0% /sys/fs/cgroup
tmpfs                        tmpfs      99M     0   99M   0% /run/user/1002
```

```s
root@caleston-lp10:~# lvs
  LV     VG          Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  data   caleston_vg -wi-a-----   1.00g                                                    
  root   vagrant-vg  -wi-ao----  <9.04g                                                    
  swap_1 vagrant-vg  -wi-ao---- 980.00m 
```



_Q7: Create an `ext4` filesystem on this logical volume and mount it at `/mnt/media`_

```shell
sudo mkdir /mnt/media
sudo mkfs.ext4 /dev/mapper/caleston_vg-data
sudo mount /dev/mapper/caleston_vg-data /mnt/media/
```

_Q8: Add `500M` to the logical volume called `data`.Do not unmount the filesystem._

```shell
sudo lvresize -L +500M -n  /dev/mapper/caleston_vg-data

sudo resize2fs  /dev/mapper/caleston_vg-data
```




প্রথমে physical volume, তারপর volume group এবং সবশেষে logical volume