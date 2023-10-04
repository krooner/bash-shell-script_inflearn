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

- mkdir (MaKe DIRectory)
- rmdir (ReMove DIRectory)
- pwd (Print Working Directory): Absolute path
- mount: output disk devices or connect to directory selected as a virtual file system
  - unmount
- stat: output statistics of selected file

## File

### 1

- touch: create empty file whose name is argument or update timestamp if a file with identical name exists
- cat (concatenate): output selected file
- head: output first N lines of selected file (default N=10)
- tail: output last N lines of selected file (default N=10)
  - `$ tail -f # f means forcely`

### 2
- cp (CoPy)

```bash
$ cp -rfp <original_file_path>/name <destination_file_path>/<name>
# r means recursive (including sub-directories)
# f means false (overwrite original file with identical name)
# p means execution permission
```
  
- mv (MoVe)
- rename

```bash
$ rename test test0 test? # <prev_pattern> <change_pattern> <target_string>
```

- rm (ReMove) `$ rm -rf`
  - 실무에서는 mv보다는 cp 후 rm


### 3

- less: output file by moving cursor
- ln (LiNk): create symbolic link (shortcut) or hard link for selected file
  - symbolic link = windows .ink file
  - `-s` option means symbolic link (if no option, hard link)
 

```bash
$ cat testfile
Hello
$ ln testfile hardlink.txt # hard link
$ cat hardlink.txt
Hello
$ echo "World" >> hardlink.txt
$ cat testfile
Hello
World
$ rm -f testfile
$ cat hardlink.txt
Hello
World
$ ln -s hardlink.txt symbolic-link.txt # symbolic link (hardlink.txt is original file)
$ mv hardlink.txt testfile
$ cat symbolic-link.txt
cat: symbolic-link.txt: No such file or directory # original file is missing!
```

### 4
- paste: read rows of selected files and merge them by tab

```bash
$ cat txt1
Hello
World
$ cat txt2
World
Wide
$ paste txt1 txt2
Hello  World
World  Wide
``` 

- dd (Dataset Definition): define dataset as block-unit and read/write file
  
```bash
# dd if=<input_file_name> of=<output_file_name> bs=<byte (size)> count=<count_block_copy>

$ dd if=/dev/urandom of=ddtest bs=1024 count=10 # 1024B = 1KB -> 10 * 1KB = 10KB
10+0 records in
10+0 records out
10240 bytes transferred in 0.000428 secs (23925234 bytes/sec)
# /dev/urandom: random character generator file provided by linux (default)
```

- tar (Tape ARchive): turn selected data or directory into a single file `tarball`
  - `.tar.gz` and `.tgg` mean compression `tarball` with gzip

```bash
# ZIP $ tar -cvzf <tar_file_name> <directory_name>/<file_name>
# UNZIP $ tar -xvzf <tar_file_name>
  # c means create
  # v means verbose (output all processes)
  # z means using gzip
  # f means explicitly indicating file name
  # x means extraction
$ tar -cvzf TV.tgz ./TV
$ tar -tf TV.tgz # list contained files
$ tar -xvzf TV.tgz # unzip 
```

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
