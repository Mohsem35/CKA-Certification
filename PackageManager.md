### Package Management Distribution

#### Introduction to Package Managers
For _Debain/Ubuntu_, it is **`apt/dpkg`** and for _CentOs/Redhat_, it is **`RPM`**

**What is a package?**

A package in its simplest defination is a **compressed archieve** that contains all the files that are **required by a particular software to run**.
    
For example: Lets consider an Ubuntu System, we want to install a simple editing system such as **`gimp`** which stands for  GNU Image Manipulation System. To do this, we can make use of the **`gimp.deb`** package which contains all the _software binaries and files_ needed to for the image editor to run along with the metadata which provides the information about the software itself.


**What is a package manager?**

A **package manager** is a software in a linux system that provides the **consistent and automated process** in _installing, upgrading, configuring and removing packages from the operating system_

#### Functions of Package Manager

- _Package integrity and authenticity_
- _Simplified package management_
- _Grouping packages_
- _Manage dependencies_

#### Types of Package Managers

`dpkg`, `rpm`, `apt`, `yum`, `apt-get`, `dnf`


### RPM and YUM

#### RPM (Redhat Package Manager)

This package manager is used in RHEL as well as other linux distributions but these are the most common ones. The File extensions for packages manage by RPM is **`.RPM`**

**Working with RPM**

RPM has **five basic modes of operations**. Each of these modes can be run using rpm command followed by a specific command options. Despite of this, **RPM doesn't resolve dependencies on its own**. This is why we make use of a higher level of package manager called **`YUM`**

1. _Installing_
2. _Uninstalling_
3. _Upgrade_
4. _Query_
5. _Verfiying_


```shell
# installation
rpm -ivh telnet.rpm

# uninstalling
rpm -e telnet.rpm

# upgrade package
rpm -Uvh telnet.rpm

# query database
rpm -q telnet.rpm

# verifying details 
rpm -Vf
```



#### YUM (Yellowdog Updater Modifier)

YUM is a free and opensource package manager.

- **Works on RPM** based Linux systems

- Works with **Software repositories** which are essentially a **collection of packages** and provides **package independency management** on RPM based distro. The repository information is stored in **`/etc/yum.repos.d/`** and repository files will have the **`.repo`** **extension**

- Acts as a **high level package manager** but under the hood it still depeneds on **`RPM`** to manage packages on the linux systems.

- Unlike RPM, YUM handles package dependencies very well (**Automatic Dependency Resolution**). It is able to install any dependencies packages to get the base package install on the linux system.




Now, lets take a look at **sequence of steps** envolve **while installing the package**

- Once yum runs `yum install` command is issued YUM **first runs transaction check**, if the package is **not installed** in the system yum **checks the configured repositories** under **`/etc/yum.repos.d/`** for the availability of the requested package.

- It also checks if there are any **dependent packages** are already installed in the system or if it needs to be upgrade.

- After this step, **transaction summary** is displayed on the screen for the user to review, if we wish to proceed with the install enter the **`y`** button
- Yum will download and **install necessary RPMs** to linux system



##### Common Commands

- To **list all the repos** added to your system.

```
yum repolist
```
- To check **which package should be installed** for specific command to work. 

```
yum provides scp
```

To **Install** a package
```
yum install httpd
```

To **remove** a package
```
yum remove httpd
```
To **update** a package
```
yum update telnet
```

- To **update all packages** in the system
```
yum update
```



_To check **how many software repositories** are configured for YUM in the system_

```
sudo yum repolist
```

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
