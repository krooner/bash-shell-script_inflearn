# Linux Commands

## File System
- cd (Change Directory): Change current working directory to indicating directory

```bash
$ pwd
/
$ cd /root/SHELL
$ pwd
/root/SHELL
$ cd - # move to previously located directory
/
```

- ls (LiSt): Output file list of current directory

```bash
$ ls -1 # vertically output file list
$ ls -a # output file list including hidden file
$ ls -l # output file list in detail
$ ls -alh # h makes easy-looking file size (K, B, M, ...)
$ ls -alt # t sorts files by revised time
$ ls -altr # r reverses order
...
$ ls -al | awk '{print $9}' # get file names in directory including hidden files 
```

```bash
$ df # 
$ df -h # h means 'human readable'
$ df -t # t includes disk type
```

## Directory and File Statistics

## File

### 1

### 2

### 3

### 4

## Process

### 1

### 2

## Network

### 1

### 2

### 3

## Search

## I/O

## ETC

## Miscellaneous
