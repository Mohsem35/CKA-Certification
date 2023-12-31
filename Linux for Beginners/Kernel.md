
Agendas

1. [Linux Core Concepts](#linux-core-concepts)
2. [Linux Boot Sequence](#linux-boot-sequence)

### Linux Core Concepts

#### Linux Kernel

If you have worked with _any operating system_, you have run into the **`term kernel`**


- The Linux kernel is **monolithic**, this means that the kernel carrries out _CPU scheduling_, _memory management_ and _several operations by itselfs_

- The Linux Kernel is also **modular**, which means it can _extends its capabilities_ through the use of _dynamically loaded kernel modules_

The Kernel is responsible for **4 major tasks**

1. **Memory** Management
2. **Process** Management
3. Device **Drivers**
4. **System calls(API) and Security**



#### Linux Kernel Versions

```shell
uname

# print the kernel version
uname -r

# print the kernel info everything 
uname -a
```


#### Kernel Space and User Space
One of the _important functions of the linux kernel_ is the **Memory Management** . We will now see how memory is seperated within the linux kernel


_Memory is divded_ into **two areas**

1. **Kernel Space**
    - Kernel _Code_
    - Kernel _Extensions_
    - Device _Drivers_
    
2. **User Space**
    - C
    - Java
    - Python
    - Ruby e.t.c
    - Docker Containers

Let us know see how programs running in the **'User Space' Work**

_All user programs_ function by _manipulating data_ that is stored in _memory and on disk_. User programs get access to data by making **special request to the kernel** called **System Calls(API)**

- Opening a file such as the **`/etc/os-release`** to see the operating system installed, results in a **`system call`**


### Working with Hardware


Lets take an example of **USB Disk** be used in the system.

- As soon as the USB device is **attached to the system** a corresponding **device driver** which is part of the **kernel space detects** the stage change and generates an **event**

- This event which is called **`uevents`** is then sent to the **`User Space`** device manager daemon called **`udev`**

- The udev service is then responsible for **dynamically creating a device node** associated with the newly attached USB drive in the **`/dev/`** filesystem.
- Once the process is complete, the newly attached disk should be **visible** under **`/dev/`** filesystem.


**Ring Buffer**: **`dmesg`** _display messages from the area of kernel_ is called Ring Buffer

When a _linux operating system boots up there were numerous messages generated by the kernel_ that _appear on the display screen_. These messages _also contain logs from the hardware devices_ that the kernel detects and provide good indication wheather it is able to configure

```shell
dmesg
dmesg | grep -i usb
```

- The **`udevadm`** is the _management utility for udev_ which **queries the database for device information**

```shell
udevadm info --query=path --name=/dev/sda5
```

- The **`udevadm monitor`** _listens to the kernel_ **new `uevents`** upon detecting an event, it prints the details such as the _device path_ and the _device name_ on the screen. This command is handy to **determine the details of the newly attached or removed device**

```shell
udevadm monitor
```


- To list all **`PCI (Peripheral Component Interconnect`**) devices that are configured in the system. Examples of PCI devices are **`Ethernet Cards`**, **`RAID Controllers`**, **`Video Cards`** and **`wireless Adaptors`** that directly attached to PCI slots in the motherboard of the computer

```shell
lspci
```




```shell
# To list information about Block Devices
lsblk
# To display about CPU information detail such as CPU architecture, cpu op-modes(32 bit, 64 bit) etc
lscpu

# To list available memory in the system
lsmem --summary
free -m

# To extract detail information about the entire hardware information of the machine
sudo lshw
```

**Total CPU(s)** = _Socket(s) x Core(s) per socket x Thread(s) per core_

_To check the exact kernel version that is running in this system_

```
uname -r
```

_what is the kernel version in 4.15.0-88-generic?_

15

_What is the command to print the messages generated by the kernel?_
```
dmesg
```


### Linux Boot Sequence

The boot process can be broken down into four stages

_BIOS POST_ -> _Boot Loader (GRUB2)_ -> _Kernel Initialization_ -> _INIT Process_

**How to initiate a linux boot process?**


First method is to **start a linux device** which is in a _halted or stopped state_

Second method is to **reboot or reset** a running system


#### 1. BIOS POST

- The first stage, called BIOS POST has very little to do with linux itself.
- POST Stands for **Power On Self Test**
- In this stage, _BIOS runs a POST test_, to ensure the **hardware components** that are attached to the device are **functioning correctly**, if POST fails the computer may not be operable and the system will not be proceed to next stage of the boot process


#### 2. Boot Loader(GRUB2)

- The next stage after BIOS POST is Boot Loader after successful of POST test.
- BIOS **loads and executes** the **boot code** from the boot device, which is located in the first sector of the harddisk. In Linux this is located in the **`/boot`** file system.
- The boot loader will provide the user with the **boot screen**, often with multiple options to boot into. Such as _Microsoft windows O.S_ or _Ubuntu 22.04 O.S_ in an example of a dual boot system
- Once the selection is made at the boot screen, **boot laoder loads the kernel into the memory** supplies it with some parameters and handsover the control to kernel
- A popular example of the boot loader is **`GRUB2`** (GRand Unified Bootloader Version 2). Its a primary boot loader now for most of the operating system.

#### 3. Kernel Initialization

- After the selected kerenl is selected and loads into the memory, it usually decompress and then loads kernel into the memory.
- At this stage, kernel carries out tasks such as **initializing hardware and memory management** tasks among other things.
- Once it is completely operational , kernel looks for **`INIT Process`** to run. Which sets up the **User Space** and the process is needed for the environment.


#### 4. INIT Process

- _In most of the current day linux distribution_, the INIT function then calls the **`systemd`** daemon.
- The **`systemd`** is **responsible** for bringing the linux host to **usable state**
- systemd is responsible for **mounting the file systems**, starting and managing **system services**
- **`systemd`** is the **universal standard these days**, but not too long ago another initialization process called _system V (five) init_ was used

To check the **`init`** system used run **`ls -l /sbin/init`**, if it is systemd then you will see a pointer to **`/lib/systemd/systemd`**

```shell
ls -l /sbin/init

lrwxrwxrwx 1 root root 20 Mar 27  2023 /sbin/init -> /lib/systemd/systemd
```








#### Questions

_1. What is the **`init process`** used by this system?_

```shell
sudo ls -l /sbin/init
```

_2. What is the **`default systemd target`** set in this system?_

```shell
systemctl get-default
```

_3. Now, change the target to **`multi-user.target`**_

```
sudo systemctl set-default multi-user.target
```

_4. You are asked to install a new **`third-party IDE`** (integrated development environment ) in the system.Which directory is the recommended choice for the installation?_

**`/opt`**

_5. Which directory contains the files related to the block devices that can be seen when running the **`lsblk`** command?_

**`/dev`**

_6. What is the name of the vendor for the Ethernet Controller used in this system?_

```shell
lspci
```