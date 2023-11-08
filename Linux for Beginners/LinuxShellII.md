Agendas

1. [File Compression and Archival](#file-compression-and-archival)
2. [Searching for files and Patterns](#searching-for-files-and-patterns)
3. [IO Redirection](#io-redirection)



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


To **archive** a file or directory

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


### Searching for files and Patterns



#### locate

Lets say you want to find the files with the name **`City.txt`**. Easiest way to do this is to make use of **`locate`** command.

```shell
locate City.txt
```

The _downside of the locate command is it depends on a database_ called **`mlocate.db`** for querying the filename. To manually update the DB, run the command **`updatedb`** and then run the locate command again

```shell
# updatedb command needs to be run as root user to work
sudo updatedb
```

#### find

Another way to do this is make use of the **`find`** command. Use the find command followed by the **directory** under which you want to **search**

```shell
find /home/ubuntu -name City.txt
```

#### Grep

To search within files, the most popular command in linux is grep.


- To search for the word **`second`** from the **`sample.txt`**

```shell
grep second sample.txt
```

```shell
# To search for the word capital with case-insensitive use -i flag
grep -i capital sample.txt

# To search for a pattern recursively
grep -r "thrid Line" /home/ubuntu

# To print the lines that don't matches the pattern
grep -v "printed" sample.txt

# To search for the whole word called exam. Use grep followed by -w flag
grep -w exam examples.txt
```



- To print the number of lines **after and before matching a pattern**. Use grep command with **`-A`** and **`-B`** flags respectively.

```shell
grep -A1 Arsenal premier-league-table.txt
grep -B1 4 premier-league-table.txt

# The -A and -B can be combined into one single search
grep -A1 -B1 Chelsea premier-league-table.txt
```



### IO Redirection

In this section, we will take a look at IO Redirection.

- IO Redirection
- Standard Streams in Linux

     

There are **three data streams** created when you launch a linux commnad

1. Standard Input (**`STDIN`**): STDIN is the standard input stream which **accepts text as an input**.
2. Standard Output (**`STDOUT`**): **Text output** is delivered as STDOUT or the standard out stream
3. Standard ERROR (**`STDERR`**): **Error messages of the command** are sent through the standard ERROR stream (STDERR)


#### REDIRECT STDOUT

```shell
# redirect STDOUT to a file instead of printing it on the screen
echo $SHELL > shell.txt

# append STDOUT to an exisiting file
echo $SHELL >> shell.txt
```

#### REDIRECT STDERR

- To redirect just the **ERROR message** we need to use **`2`** followed by forward arrow **`>`** symbol and then the name of the filename in which the errors are written.

```shell
cat missing_file 2> error.txt

# append the STDERR to the exisiting file
cat missing_file 2>> error.txt
```


- If you want to execute and **not print ERROR messages** on the screen even if it generates a standard ERROR.

```shell
cat missing_file 2> /dev/null
```

##### Questions

_Q: Create a tarball of the directory called python and compress it using gzip. The compressed tar file should be available at `/home/bob/python.tar.gz`_

```shell
tar -cf /home/bob/python.tar /home/bob/reptile/snake/python
gzip /home/bob/python.tar
```

_Q: Find the location of the file called dummy.service and redirect its absolute path to the file /home/bob/dummy-service. You can use the redirect operator with the echo command to save the answer to the file._

```shell
sudo find / -name dummy.service
echo /etc/systemd/system/dummy.service > /home/bob/dummy-service 
```


_Q: Find the file under `/etc` directory that contains the string `172.16.238.197`. Save the answer using the absolute path in the file `/home/bob/ip`_

```shell
sudo grep -ir 172.16.238.197 /etc/ > /home/bob/ip
```

_Q: Create a new file called `/home/bob/file_with_data.txt`. This file should have one line of text that says `a file in my home directory`_

```shell
echo 'a file in my home directory' > /home/bob/file_with_data.txt
```

_Q: Run the command `python3 /home/bob/my_python_test.py` and redirect the standard error to the file `/home/bob/py_error.txt`_

```shell
python3 /home/bob/my_python_test.py 2> /home/bob/py_error.txt
```

_Q: Read the file `/usr/share/man/man1/tail.1.gz` and without extracting redirect the contents to a file called `/home/bob/pipes`_

```shell
zcat /usr/share/man/man1/tail.1.gz > /home/bob/pipes
```