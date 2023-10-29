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