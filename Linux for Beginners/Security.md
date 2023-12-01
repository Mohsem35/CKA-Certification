### LINUX ACCOUNT

Linux security is divided in many partrs, some of them are

1. **`Access Controls`**: Access control methods make use of **user and password-based authentication** to determine who can access the systems

2. **`PAM`**: Stands for **Pluggable Authentication Model** and it is another way of managing authentication in Linux. It is normally used to **authenticate users to programs and services** in Linux.

3. **`Network Security`**: Restrict or allow access to **services listening** on the Linux server. While we commonly rely on external firewalls to do this,
it can also be set up within the Linux system by making use of tools such as **IPTables** and **Firewalld**
4. **`SSH Hardening`**: SSH stands for Secure Shell
and it is used for **remote access to a server over an unsecured network**. 
5. **`SELinux`**: SELinux makes use of **security policies for isolating applications** running on the same system from each other
to protect the Linux server


- **User's informations** are stored under /etc/passwd file.

```shell
cat /etc/passwd
```
```shell
# maning of the output
ubuntu:x:1000:1000:Asif Yusuf:/home/ubuntu:/bin/bash

USERNAME:PASSWORD:UID:GID:USER INFO:HOME DIRECTORY:DEFAULT SHELL
```


ubuntu = user

x = means, that **encrypted password** is stored in `/etc/shadow` file

1000 = UID(unique id)UID, **0 is reserved for root**

1000 = GID(group id)All users in a group will have same GID

Asif Yusuf = user info

`/home/ubuntu` = user home directory

`/bin/bash` = user default shell


- **Information about groups** is stored into /etc/group file.
```shell
cat /etc/group
```


### ACOUNT TYPES

1. User account: bob, machael, dave
2. Superuser account: root. UID = 0
3. **System accounts**: software and services. ssh, mail. UID < 100 OR between 500-1000
4. **Service accounts**: services installed in linux. nginx



- To see the list of users **currently logged use** who command.
```shell
who

# output
bob pts/2 Apr 28 06:48 (172.16.238.187)
```

- Display the **record of all logged-in users** along with the date and time when the system was rebooted

```shell
last

# output
michael :1 :1 Tue May 12 20:00 still logged in
sarah :1 :1 Tue May 12 12:00 still running
reboot system boot 5.3.0-758-gen Mon May 11 13:00 - 19:00 (06:00)
```

- Users listed in /etc/sudoers file can make use of **sudo command for privledge** escalation.

```shell
cat /etc/sudoers
```

- To **restrict anyone from directly login as root login**, this can be done by setting nologin shell

```shell
sudo nano  /etc/ssh/sshd_config

# change the line
PermitRootLogin no

# restart the service
sudo systemctl restart sshd
```


### ACCESS CONTROL FILES

- **Password are stored** under `/etc/shadow`

```shell
grep -i bob /etc/shadow

# output
bob:$6$0h0utOtO$5JcuRxR7y72LLQk4Kdog7u09LsNFS0yZPkIC8pV9tgD0wXCHutY
cWF/7.eJ3TfGfG0lj4JF63PyuPwKC18tJS.:18188:0:99999:7:::

USERNAME:PASSWORD:LASTCHANGE:MINAGE:MAXAGE:WARN:INACTIVE:EXPDATE
```

- Check the **groups bob belongs too**

```shell
grep -i bob /etc/group
NAME:PASSWORD:GID:MEMBERS
```


### USER MANAGEMENT

**User Add**

- To **create a new local user** bob in the system use useradd command.
```shell
useradd bob
```

useradd command be used **along with many attributes** as show below

```shell
useradd -u 1009 -g 1009 -d /home/robert -s /bin/bash -c ”Mercury Project member" bob

# meaning of options
-c custom comments
-d custom home directory
-s specify login shells
-g specify GID
-u specify UID
-G create user with multiple secondary groups
```

user, group এইসব আগে থাকব, পরে `del`, `add` এইসব word পরে যুক্ত করব 

- To **check the uid or username of the user logged in** user whoami command.
```shell
whoami
```

- To **delete a user** use userdel command
```shell
userdel bob
```

- To add a group use groupadd command
```shell
groupadd –g 1011 developer
```

- To **delete a group** user groupdel command
```shell
groupdel developer
```

_Q1: Which of the following commands will show you the UID for a user?_

id

_Q2: Which access control file has the encrypted password for the users?_

`/etc/shadow`

_Q3: A user called chris has been created. Can you find out his Full Name?_

```shell
grep -i chris /etc/passwd
```

_Q4: Create a group called john with the GID 1010. Next create another user called john with UID = 1010, primary group = john and login shell = /bin/sh_

```shell
sudo groupadd -g 1010 john
sudo useradd -u 1010 -g 1010 -s /bin/sh john
```


### LINUX FILE PERMISSIONS & OWNERSHIP

```shell
ls -l template_test.sql 

# output
-rw-rw-r-- 1 ubuntu ubuntu 29135090 Nov 21 15:43 template_test.sql
```

| File type  | Identifier |
| ------------- | ------------- |
| directory  | d  |
| regular file  | -  |
| character device  | c  |
| link  | l  |
| socket file | s  |
| pipe  | p |
| block device | b |



`u` `g` `o`

আগে user, তারপর group, সবশেষে others

**Octate values**


4 = Read

2 = Write

1 = Execute



#### Modifying file permissions

<img width="550" alt="Screenshot 2023-12-01 at 6 16 07 PM" src="https://github.com/Mohsem35/CKA-Certification/assets/58659448/a2276157-0d74-4491-bbba-2ff096f3f2f3">


ফাইলের permission change করা, আর ownership change করা ২ টা ২ জিনিষ 

Use `chmod` command to modify the file permissions.

আগে user, group and other এর শর্টফর্ম থাকব, তারপর `-` sign দিয়া permissions গুলো বসাইতে হইব 

1st approach

```shell
# provide full access to owners
chmod u+rwx test-file

# provide Read access to owners, groups and others, Remove execute access
chmod ugo+r-x test-file

# remove all access for others
chmod o-rwx test-file

# full access for Owner, add read , remove execute for group and no access for others
chmod u+rwx,g+r-x,o-rwx test-file
```

2nd Approach (Follow this)

```shell
# 4 = read, 2 = write, 1 = execute

# provide full access to owners, group and others
chmod 777 filename

# provide read and execute access to owners,groups and others
chmod 555 filename

# read and write access for Owner and Group, No access for others.
chmod 660 filename

# full access for owner, read and execute for group and no access for others.
chmod 750 filename
```

#### Change Ownership

```shell
# changes owner to bob and group to developer
# chown <username>:<groupname> <file_name>
chown bob:developer filename


# changes just the owner of the file to bob. Group unchanged.
# chown username filename
chown bob andoid.apk


# change the group for the test-file to the group called android.
chgrp android filename
```




