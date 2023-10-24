Agendas

1. [Introduction to Shell](#introduction-to-shell)
2. [Basic Linux Commands](#basic-linux-commands)

### Introduction to Shell


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

`type`

_In the command “echo -n hello”, what is “echo”_

command


### Basic Linux Commands

To recursively created directories

```
mkdir -p <directory_name>/<sub_directory_of_name>
```

##### Difference Between Absolute and Relative Path

_Absolute Path_: An absolute path is defined as specifying the location of a file or directory **`from the root directory(/)`**.

_Relative Path_: Relative path is defined as the path related to the present working directly(**`pwd`**).


To copy a directory recursively
```
cp -r <sourcepath> <destinationPath> command
```

To add a content to a file with cat(redirect)

```
cat > /path/to/<filename>
```

**Pagers**

The **`tail`** command is used to display the last few lines of a text file

```
tail -n 10 -f filename
```


To list all the files form oldest to newest

```
ls -ltr
```