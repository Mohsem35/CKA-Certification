### File Types in Linux

Everything is a file in Linux. **Every object** in linux can be considered to be a type of **file**, even a directory for example is a special type of file.

There are three types of files.

1. _Regular File_
2. _Directory_
3. _Special Files_


**Special files** are again catagorized into **five** other file types.


1. Character Files
    - These files represent **devices** under the **`/dev`** file system.
    - Examples include the devices such as the **keyboard** and **mouse**
    
2. Block Files
    - These files represent **block devices** also located under **`/dev/`** file system.
    - Examples include the harddisks and **RAM**
    
3. Links
    - Links in linux is a way to associate two or more file names to the same set of file data.
    - There are two types of links
        - The **Hard Link**
        - The **Soft Link**
4. Sockets
    - A sockets is a special file that enables the communication between two processes.

5. Named Pipes
    - The Named Pipes is a special type of file that allows connecting one process as an input to another


One way to identify a **file type** is by making use of the **`file`** command

```shell
file /home/ubuntu
file bash-script.sh
file test.sql

# output
/home/ubuntu: directory
```

Another way to identify a file type is by making use of the **`ls -ld`** command

### Filesystem Hierarchy



**`/opt`** : If you want to **install any third party programs** put them in the /opt filesystem.

/mnt : It is the default mount point for any partition and it is empty by default. It is used to mount filesystems temporarly in the system

**`/tmp`** : It is used to **store temporary data**

**`/media`** : **All external media is mounted** on /media

**`/dev`** : Contains the special **block and character device files**

**`/bin`** : The basic programs such as **binaries** cp, mv, mkdir are located in the /bin directory

**`/etc`** : It stores most of the **configuration files** in Linux.

**`/lib`** : The directory /lib and /lib64 is the place to look for **shared libraries to be imported into your program**

/usr : In older systems, /usr directory is used for User Home Directories, however in the modern linux operating systems it is the location where all user land applciations in their data reside

**`/var`** : It contains variable data like mails, **log files and cached data**

- To print all the **mounted filesystems**, run **`df`** (disk filesystem) command
```
df -hPT
```