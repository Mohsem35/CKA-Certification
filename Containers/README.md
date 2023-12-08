
What can Docker do? 

- Containerize applications
- Run each service with its own dependencies in seperate containers


**Windows** runs a Linux container on a Linux virtual machine under the host. So. it's really Linux container on **Linux virtual machine** on Windows

**Unlike hypervisors**, Docker is not meant to virtualize and run different OS and kernels on the same hardware. The main purpose of Docker is to **package and containerize applications**, and to **ship them and to run them** anywhere, anytime, as many times as you want.

An **`image`** is a package or a **template** just like a VM template in virtualization world. It i is used to create one or more containers.

**`Containers`** are **running instances of images** that are isolated and have their own environments and set of processes