Service management with **systemd**


### SYSTEMD Service


**`Systemd`** is a Linux initialization system and **service manager** that includes features like on-demand **starting of daemons**, **mount** and **automount point maintenance** etc.
Systemd also provides a **logging daemon** and other tools and utilities to help with common system administration tasks.

What is a service unit?

A file with the `.service` suffix contains **information about a process** which is managed by systemd. It is composed by three main sections:

1. **`Unit`**

- The unit section of a .service file contains the **description of the unit itself**, and information about its behavior and its dependencies: (to work correctly a service can depend on another one). Here we discuss some of the most relevant options which can be used in this section

- **`Description`** option. By using this option we can provide a description of the unit. The description will then appear, for example, when calling the systemctl command, which returns an overview of the status of systemd.

- **`Documentation`** option. By using this option we can get the **details of the service and documentation** related to it.

- **`After`** option, we can state that our **unit should be started after the units we provide** in the form of a space-separated list.

```shell
cat /etc/systemd/system/project-mercury.service
```
```
[Unit]
Description=Python Django for Project Mercury
Documentation=http://wiki.caleston-dev.ca/mercury
After=postgresql.service
```


2. **`Service`**

In the Service section of a service unit, we can specify things as the command to be executed when the service is started, or the type of the service itself.

```shell
[Service]
ExecStart=/usr/bin/project-mercury.sh
User=project_mercury
Restart=on-failure
RestartSec=10
```

3.**`Install`**

This Install section contains information about the installation of the unit
```shell
[Install]
WantedBy=graphical.target
```


**How to Start the Service now ?**

The system to detect the changes you have done in the file, we need to reload the daemon and start the service.

```shell
systemctl daemon-reload
systemctl start project-mercury.service
```

### SYSTEMD Tools to Manage SYSTEMD service

#### SYSTEMCTL

`Systemctl` is the **main command** used to manage services on a SYSTEMD managed server. It can be used to manage services such as `START/STOP/RESTART/RELOAD` as well as `ENABLE/DISABLE` services **during the system boot**.

```shell
# To start a service use the start command
systemctl start docker

# To stop a service use the stop command
systemctl stop docker

# To restart a service use the restart command
systemctl restart docker

# To disable a service at boot use the disable command
systemctl disable docker

# To know the status of the service use systemctl status docker command
systemctl status docker
```
**Important**
```shell
# To reload a service, it will reload all the configuration without interrupting the normal functionaltiy of the service
systemctl reload docker

# system reboot হওয়ার সাথে সাথে যদি service restart করতে চাই তবে 
systemctl enable docker
```



- Running systemctl **daemon reload** command **after making changes to service unit file** reloads the system manager configuration and makes the systemd aware of the changes.

daemon alerted হয়ে যাবে for the service  
```shell
systemctl daemon-reload
```

- To edit the service file use command `systemctl edit project-mercury.service --full` this will open a **text editor**, you can make the changes and re-write the settings as needed, making changing this way **applied immediately** without running the systemctl daemon reload command

service কে যদি direct edit করতে চাই তবে edit options তে চলে যাব 
```shell
systemctl edit project-mercury.service --full
```

- To see the **current runlevel** use systemctl get-default
```shell
systemctl get default
```
- To **change the runleve** to a different target use systemctl set-default multi-user.target
```shell
systemctl set-default multi-user.target
```
- To **list all the units** that systemd has loaded, this lists all the unit which are active, inactive or anyother state.
```
systemctl list-units --all
```
- To **list only active units** 
```shell
systemctl list-units
```
- To **view**, and also locate a unit file. A comment line containing the path to the unit file is printed as the first line of output.
```shell
systemctl cat project-mercury.service
```


#### JOURNALCTL

Journalctl is a command for **quering/viewing logs collected by systemd**. The systemd-journald service is responsible for systemd’s log collection, and it retrieves messages from the kernel systemd services, and other sources. Very useful when you are **troubleshooting issues** with systemd services.


```shell
# print all the log entries from oldest to the newest
journalctl

# print all the logs from the current boot
journalctl -b

# print all the logs specific to the unit specified
journalctl -u docker.service

# --since command print all the logs specific to the unit specified since the given time]
journalctl -u docker.service --since "2022-01-01 13:45:00"
```