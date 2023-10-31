### DPKG and APT 

#### DPKG Utility

- DPKG stands for Debian Package Manager
- It is a **low level package manager**

```shell
# installation / upgrade dpkg package
dpkg -i telnet.deb

# uninstalling
dpkg -r telnet.deb

# dpkg repository list
dpkg -1 telnet

# status
dpkg -s telnet

# verifying details
dpkg -p
```

> _Note:_ Similar to RPM, DPKG **doesnt resolve the dependencies** when it comes to package management.

#### APT and APT-GET

**Instead of relying on DPKG**, you can install software along with its dependencies using **`APT`** or **`APT-GET`**

**`APT`** stands for **advanced package managers**

APT act as a frontend package manager that **relies on DPKG utility**. In similar to YUM, APT relies on software repository that contains packages that would eventually be installed on a system.

- The software repository for APT is defined in **`/etc/apt/sources.list`** file.


##### Common commands

- To **refresh a repository**
```
sudo apt update
```

- To **install available upgrades** of all packages currently installed on the system from the sources configured
```shell
sudo apt upgrade
```

Another way to update the repository is to use **`apt edit-sources`** command. This opens up the **`/etc/apt/sources`**.list file in the text editor of your choice.
```shell
sudo apt edit-sources
```

To **install** the package
```shell
sudo apt install telnet
```

To **remove** the package
```shell
sudo apt remove telnet
```

- To **search/look for a package** in the repository
```shell
sudo apt search telnet 
```
- To **list all available packages**
```shell
sudo apt list | grep telnet
```

**Difference between APT vs APT-GET**

- APT is a **more user friendly** tool when compared to APT-GET
- In all the **latest debian** based distros APT is already installed by default.
