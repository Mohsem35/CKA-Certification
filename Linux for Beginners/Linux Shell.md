

###


Command line interface (**`CLI`**) will enable you to effectively work on linux laptop/server/virtual machine.


**What is a shell?**

Linux shell is a program that allows **`text-based interaction`** between user and OS. The interaction is carried out by _typing commands into the interface_


```
# how long a system has been running for since the last reboot along with the other information
uptime
```

#####  Command Types

1. _Internal or Built-in Commands_: Internal commands are part of the shell itself. `echo`, `cd`, `pwd`, `set` e.t.c.

2. _External Commands_: External commands on the other hand are binary programs or scripts which are usually located in distinct files in the system. `mv`, `date`, `uptime`, `cp` e.t.c.


To determine a command is internal or external, use **`type`** command

```
$ type echo
echo is a shell built-in 
```

```
$ type mv
mv is a hashed (/bin/mv)
```
_Which symbol represents a user’s home directory in Linux?_

~

_In the command “echo -n hello”, what is “-n” ?_

option

_Which command would you use to find out the type of a command?_

type

_In the command “echo -n hello”, what is “echo”_

command