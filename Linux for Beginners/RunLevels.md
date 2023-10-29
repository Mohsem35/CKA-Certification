### Run Levels


#### Systemd Targets (Run Levels)

We can setup the server to boot either into graphical mode or non-graphical mode. _Linux can run in multiple modes_ and these **modes** are set by something called **`runlevel`**

- The operation mode which provide a **graphical** interface is called **`runlevel 5`**
- The operation mode which provide a **non-graphical** mode is called **`runlevel 3`**


To see the **operation mode** run in the system
```shell
# command
runlevel

# output
N 5
```

In the boot process section, we saw that the **`systemd`** is used as the init process in most new linux distributions suchs as _Ubuntu 22.04_

In **`systemd`**, **runlevels** are called as **targets**


```shell
# command
systemctl get-default

# output
graphical.target
```

- Now that we are familiar with runlevels in systemd target unit. Lets now take a look, how we can change from **graphical to non-graphical** or vice-versa from a shell.

To change the default target, we can make use of 

```shell
# example
systemctl set-target <desired target name goes here as an argument>
```

```shell
# converts from graphical to non-graphical mode
systemctl set-default multi-user.target 
```

```
The complete list of runlevels and the corresponding systemd targets can be seen below:

runlevel 0 -> poweroff.target
runlevel 1 -> rescue.target
runlevel 2 -> multi-user.target
runlevel 3 -> multi-user.target
runlevel 4 -> multi-user.target
runlevel 5 -> graphical.target
runlevel 6 -> reboot.target
```