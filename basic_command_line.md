# File management and directories



### Directories
File and directory paths in UNIX use the forward slash "/"
to separate directory names in a path. If you are using a server your shell will start from /home/yourUserName/ directory. Also have a look at the conventional directory layout [here](http://en.wikipedia.org/wiki/Unix_filesystem#Conventional_directory_layout).

Here are a few examples of the directory strcucture:

| directory | explanation |
| -- | -- |
| `/` | "root" directory |
| `/usr`  | directory usr (sub-directory of / "root" directory)|
| `/usr/local` | local is a subdirectory of /usr |

#### Creating a directory

**mkdir** command creates a new directory. The command below creates a new directory named "newDir" under the current directory.
```
$ mkdir newDir
```

This command creates a new directory in user's home directory.
```
$ mkdir ~/newDir
```

The next command creates a the target directory and all the non-existing directories in the path. The command will create samtools directory, and will create "opt" directory if it does not exist. All of this will be done in user's home directory as indicated by "~/" in that path.

```
$ mkdir -p ~/opt/samtools
```

### Moving around the file system with cd
***cd*** command stands for "change directory" lets you move around the file system. Here are a few examples of the ***cd*** command and ***pwd***.

Type variants of these to your shell to move around your file system.

| Command + arguments| explanation |
| -- | -- |
| `pwd`  |Show the "present working directory", or current directory. |
| `cd` | Change current directory to your HOME directory. |
| `cd /usr/local` |Change current directory to /usr/local  |
| `cd INIT` | change current directory to INIT which is a sub-directory of the current |
| `cd ..` | Change current directory to the parent directory of the current|
| `cd ~akalin`| Change the current directory to the user akalin's home directory (if you have permission).|



### Listing directory contents
***ls*** command lists the contets of a directory. It can take multiple options, some of those are explained below.

| commands | explanation |
| -- | -- |
| `ls`| list a directory |
| `ls -l` | list a directory in detailed format including **file sizes** and **permissions** |
| `ls -a` | List the current directory including hidden files. Hidden files start with "." |
| `ls -ld *` | List all the file and directory names in the current directory using  long format. Without the "d" option, ls would list the contents of any sub-directory of the current. With the "d" option, ls just lists them like regular files. |
| `ls -lh ` | list detailed format this time file sizes are human readable not in bytes |




### Moving, renaming and copying files
***cp*** command **copies** the files and ***mv*** command **moves** the files. They are generally used with two main arguments. `cp target_file destination_file` or `mv target_file destination_file`.

| commands | explanation |
| -- | -- |
|`cp file1 file2`|          copy file1 as file2
|`cp /data/seq_data/file1  ~/`|          copy file1 at /data/seq_data to your home directory.
|`mv file1 newname` |        move or rename a file|
|`mv file1 ~/opt/` |         move file1 into sub-directory opt in your home directory.|

### Finding files
There are a couple of ways you can find files in your file system. We will show the **find** command, it works in the following syntax `find directory -name targetfile`. It is useful when you have a rough idea about file location.

The following finds all files ending in ".html" under /home/user directory.
```
$ find /home/user -name "*.html"
```
find can also do more than just finding files. It also execute commands on the files you find via -exec option. The following command finds all files in the current directory with ".txt" ending and counts the number of lines in every text file. The '{}' is replaced by the name of each file found and the ';' ends the -exec clause.

```
$ find . -name "*.txt" -exec wc -l '{}' ';'

```

Another command that can find files is **locate**.
The locate command provides a faster way of locating all files whose names match a particular search string. For example:
```
 $ locate ".txt"
```
will find all filenames in the filesystem that contain ".txt" anywhere in their full paths.

A disadvantage of locate is that it stores all filenames on the system in an index that is usually updated only once a day. This means locate will not find files that have been created very recently.

### Searching the contents of a text file
Often times you would need search a file for existence of certain characters or words. Imagine that you need to find gene ids in a text file containing some scores and gene ids, you would like to get the line(s) that contains your gene id of interest. This is similar to "find" functions in modern text processors such as MS Word. This can be achieved via **grep commmand**. Syntax of the command is: `grep options pattern files`

| Command | Explanation |
| -- | -- |
| `grep id1 genes.txt` | searches and prints lines matching "id1" in "genes.txt" |
| `grep id1 *.txt` | searches and prints lines matching "id1" in files ending with ".txt" |
|`grep -vi id1 *.txt`   |  similar to above, but -i option ignores the case (Id1,ID1,iD1 and id1 treated equally), -v option prints lines that don't match the pattern   |

#### using grep and find together
You can search all files in an entire directory tree for a particular pattern by combining **grep** and **find**. The following command prints lines containing "genes" string, from the files 'find . -name "*.txt" -print' found.

    $ grep genes `find . -name "*.txt" -print`

The search patterns that grep uses are a special
named **regular expressions**. You can have more comlicated searches using regular expressions, but that is more of an advanced application. See [] for more on that.

### See disk usage and free disk space with du and df

### Deleting files and directories
Use these with caution.

# Compressing and decompressing files

UNIX systems usually support a number of utilities for backing up and compressing files. Below we will show tar,gzip and zcat utilities

##tar (tape archiver)
tar backs up entire directories and files as an archive. An archive is a file that contains other files plus information about them, such as  their filename, owner, timestamps, and access permissions. tar does not perform any compression by default.

To create a disk file tar archive, use

    $ tar -cvf archivename filenames

where archivename will usually have a .tar extension, so `tar -cvf archivename.tar filenames`. The *c option* means create, the *v option* means verbose (output filenames as they are archived), and *option f* means file.To list the contents of a tar archive, use

    $ tar -tvf archivename

To restore files from a tar archive, use

    $ tar -xvf archivename


## compressing files with gzip
gzip is a utility for compressing and decompressing individual files. To compress files, use:

    $ gzip filename

The filename will be deleted and replaced by a compressed file called filename.Z or filename.gz. To reverse the compression process, use:

    $ gzip -d filename

## viewing compressed text files with zcat
zcat uncompresses the file and writes content to the standard output (which usually means the output will be on your shell screen). The following decompresses the files and writes the content on the standard output.

 `$ zcat geneList.gz`

or in some systems:

 `$ gzcat geneList.gz`

If the file is big and you just want to see what is in it (first few lines), you can combine zcat with **head**, you will see more of this in the next section, here is an example.

 `$ zcat geneList.gz | head`

"|" allows piping commands together. **head** command is applied on the output of **gzcat** command.

# Editing, viewing and concatenating text files
In many cases, you would want to see what is in a text file or edit a file, or create one from scratch. There are multiple ways to view the files.


### cat : print the content of a file
`cat` will print the whole content of the text file to your shell, if you don't redirect the output to another file. Here is how it works:
```
$ cat log.txt
AD 123
AA 233
Ad 443
$
```

### head or tail: Print beginning or end of a file
If you would like to display only the beginning of a file or the end, you can use `head` or `tail`. `head` shows the beginning and `tail` the end. Here we look at beginning and the end of `example.fastq` file, which contains reads from a next-generation sequencing experiment.

```
$ head example.fastq
@DJG60NM1:206:C2L3LACXX:1:1101:1934:1996 1:N:0:GCCAAT
NTTTGTAGAACCTTTTGTAATTCTGCTTATATTGGTAGCCAATGCAATTGT
+
#1=DFFFFHHHHHIJJIIIIJJFIJJJJIJJJJIIIJJIJJIJIJJIIJJG
@DJG60NM1:206:C2L3LACXX:1:1101:2193:1996 1:N:0:GCCAAT
NGAATCACCCCTTTAAATCCCCTAGAAGTACCCCTTCTAAATACATCAGTC
+
#1=DDDFFHHHHHJIIJIJIJJGIGIHI?FHGIJIJIJIJIFIEEIGJBBE
@DJG60NM1:206:C2L3LACXX:1:1101:2277:1999 1:N:0:GCCAAT
NTTTAATAATGCGCTCCCCAAACAGAACAGCACCACCACTGGGGTCTCCGT
$
$
$ tail example.fastq
@DJG60NM1:206:C2L3LACXX:1:1101:2023:1996 1:N:0:CAGATC
NTTGAAGTTAAATTTGGGAACTCATAGGAAATAAAGGAGCATGAACCAAGG
+
#1=DDDFFHHHHHJJIJJIJJJJIJJJJIIJJJJJJJJJJJJIJJJJJJJI
@DJG60NM1:206:C2L3LACXX:1:1101:2057:2000 1:N:0:CAGATC
NGGTTGGTGGGCTGATGTCTATAAGTACTAGGGTAGCTCCTCCGATTAGAT
+
#1:4ABDDDDD?D@DEEEBFFFEIEEBBEDD;BC?DEDEBBDDD6BD@B8B
@DJG60NM1:206:C2L3LACXX:1:1101:2305:1999 1:N:0:CAGATC
NGTATTTCATGTGGTATAAGCATCTGGATAATCAGAGTAACGACGAGGTAT
```

### more and less: Read files page by page
In some cases, you may want to look at a large file page by page. If you use `cat`, the whole file will be printed at once. It is inconvenient to use that if you want to inspect a large file. Instead you can use `more` or `less`, to view the files page by page.


### creating text files with echo
`echo` command prints the argument on the shell. See the following example.

```
$ echo hello world
hello world
$
$ echo here it is
here it is
```
However, you can also use echo to write files using `>`. When you finish typing just add `> filename.txt` and the output will be written on `filename.txt`

```
$ echo hello world > myfile.txt
$
$ cat myfile.txt
hello world
$
```
