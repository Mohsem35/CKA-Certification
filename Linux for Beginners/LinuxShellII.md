### File Compression and Archival

The **`du`** command, which stands for **disk usage** is a popular command to inspect the size of the file.

#### Viewing file sizes

Size of a file or directory in **human readable format**

```shell
# directory size
du -sh /home/ubuntu

# file size
du -sh /home/ubuntu/test_image.png
```

#### Archiving Files

- **`tar`** is used to **group multiple files and directories into a single file**. Hence it is specially used for archiving data.
- tar is an abrevation for **tape archive**.
- Files created with tar are often called tarballs.


To archive a file or directory. 

```shell
tar -cf test.tar file1 file2 file3 
ls -ltr test.tar
```

**`-c`** = to _create an archive_
**`-f`** = is used to _specify the name of the tar file_ to be created. 
**`test.tar`** = These is followed by _files or directories_ to be archive.

Useful commands for tarball 

```shell

# see the contents of the tarball
tar -tf test.tar
```
```shell
# extract the contents from the tarball
tar -xf test.tar
```
```shell
# compress the tarball to reduce its size
tar -zcf test.tar
```


#### Compression

Compression is the technique used to **reduce the size** consumed by a file or a dataset.

To reduce the size of a file or directory in the linux file system, there are commands specificly used for compression.
Let us now look at the three popular ones

1. _bzip2_ (.bz2 extension)
2. _gzip_ (.gz extension)
3. _xz_ (.xz extension)

```shell

# compression commands
bzip2 test.img
gzip test1.img
xz test2.img
```

The compressed files can be **`uncompressed`** by using the below commands

- bunzip2
- gunzip
- unxz

```shell

# decompression commands
bunzip2 test.img
gunzip test1.img
unxz test2.img
```


The space of the compressed files created by these three commands depends on a few factors, such as the type of data being compressed, the other factors that effect the size are the compression algorithm used by these commands and the compression level used.
The compressed files can be uncompressed by using the below commands


#### Compressed files need not to be uncompressed everytime

- Tools such as **`zcat`** , bzcat and xzcat allow the compressed files to be **read without an uncompress**

```shell
zcat hostfile.txt.bz2
zcat hostfile.txt.gz
zcat hostfile.txt.xz
```

