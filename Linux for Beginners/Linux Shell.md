Agendas

1. [Introduction to Shell](#introduction-to-shell)
2. [Basic Linux Commands](#basic-linux-commands)
3. [Command Line Help](#command-line-help)
4. [Bash Shell](#bash-shell)
5. [Questions](#questions)

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
type echo
echo is a shell built-in 
```

```
type mv
mv is a hashed (/bin/mv)        # file type command
```


### Basic Linux Commands

Check home directory of current user 
```
echo $HOME
```

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

To add a content to a file with cat (redirect)

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


### Command Line Help

First command is **`whatis`** , this command will displays a one line description of a command does

**`Syntax: whatis <command>`**
```
whatis date
```

Most of the commands internal or external come bundled with **`man pages`** which provides information about the command in detail (with examples, usecases and with command options)

**`Syntax: man <command>`**
```
man date
```
Several commands will provide **`-h`** or **`--help`** to provide users with the options and usecases available in a command
```
date -h
date --help
```

To search through the man page names and descriptions for instances of the keyword use **`apropos`**. This is useful if you want to look for all _possible commands within the system_ that contains the **`specifc keyword`**

**`Syntax: apropos <keyword>`**

```
apropos modpr
```

_Q: In the command `echo Welcome`, what does the word `Welcome` represent with respect to the command?_

argument

_Q: To check the home directory for a particular user say **bob**_

```shell
grep bob /etc/passwd | cut -d ":" -f6
```

### Bash Shell

There are different types of shells in linux, some of the popular ones are below

- Bourne Shell (sh)
- C Shell (csh or tsh)
- Korn Shell (ksh)
- Z Shell (zsh)
- _Bourne again shell (Bash)_


To check which shell being used

```shell
echo $SHELL
```

To **`change the default shell`**. Use the command **`chsh`**, you will be prompted for the password and following that _input the name of the new shell_. You have to login into new terminal session to see this change though.
```
chsh
```

**Bash Shell Features**

1. Bash supports command _auto-completion_
2. Can set _custom aliases for the actual commands_

```
date
alias dt=date
dt
```

3. Use the **`history`** command to list the previous run _commands that you ran earlier_


**Bash Environment Variables**


To see a list of all environment variables
```shell
env
```

To set an environment variable we can use the **`export`** command. To make the value carry forward to any other process.
```
export OFFICE=caleston
```

To **`persistently`** set an environment variable over subsequent login or a reboot add them to the **`~/.profile`** or **`~/.pam_environment`** in the users home directory

```shell
echo "export OFFICE=caleston" >> ~/.profile (or)
echo "export OFFICE=caleston" >> ~/.pam_environment
```


**Path Variable**

When a _user issues an external command into the shell_, the _shell uses path variable to search for these external commands_

To see the directories defined in **`path variable`**

```shell
echo $PATH
```

To check if the **`location`** of the command can be **`identified`**

**`Syntax: which <command>`**
```
which obs-studio
```

**Customize Bash Prompt**

Once you login into the shell, the line you see is the **`bash prompt`**

```shell
[~]$
```

The bash prompt is set in control by a set of special shell environment variables. The most common of these and the one we will focus on is **`PS1`** variable
```bash
echo $PS1
```

To change the bash prompt to display **`date`**, **`time`**, **`username of the current user`**, the **`hostname`** and the **`current working directory`**

```bash
PS1="[\d \t \u@\h:\w ] $ "
echo $PS1
```
Special characters for further customize bash prompt
```
\d: the date in "Weekday Month Date" format (e.g., "Tue May 26") le: an ASCII escape character (033)
\h: the hostname HQDN
\H: the complete hostname
\n: newline
\r: carriage return
\s: the name of the shell
\t: the current time in 24-hour HH:MM:SS format 
\T: the current time in 12-hour HH:MM:SS format 
\@: the current time in 12-hour am/pm format 
\A: the current time in 24-hour HH:MM format
lu: the username of the current user
\w: the current working directory, with $HOME abbreviated with a tilde
\W: the basename of the current working directory, with $HOME abbreviated with a tilde
\$: if the effective UID is 0, a #, otherwise a $
```




### Questions

_1. Which symbol represents a user’s home directory in Linux?_

~

_2. In the command “echo -n hello”, what is “-n” ?_

option

_3. Which command would you use to find out the type of a command?_

`type`

_4. In the command “echo -n hello”, what is “echo”_

command

_5. To change the shell for bob from **`Bash`** to **`Bourne Shell`**_
```shell
chsh -s /bin/sh bob
```

_6. What is the value of the environment variable TERM_

```
echo $TERM
```

_7. Create a new environment variable called **`PROJECT=MERCURY`** and make it persistent by adding the variable to the **`~/.profile`** file_

```
echo "export PROJECT=MERCURY" >> ~/.profile
```

_8. Set an **`alias`** called up for the command uptime and make it **`persistent`** by adding to **`~/.profile`** file_
```shell
echo alias up=uptime >> ~/.profile
```

_9. Update Bob's prompt so that it displays the date as per the format below: Example: **`[Wed Apr 22]bob@caleston-lp10:~$`** Make sure the change is made persistent_

```shell
PS1='[\d]\u@\h:\w\$'
or
echo 'PS1=[\d]\u@\h:\w$' >> ~/.profile
```

