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

- ps (Process Status): Output information of currently running process in system

> Process
> - 프로그램 실행 시, 프로그램 자체와 작업 내용이 메모리에 올라가서 실행되는 작업 단위
> - 실행할 내용을 메모리에 올려놓고, CPU는 메모리에서 실행할 내용을 읽어다가 실행을 하는 방식

```bash
$ ps -ef
# UID means username who runs the process
# PID means process ID
# PPID means parent process ID
# STIME means start time
$ ps aux
# prints %CPU, %MEM
# VSZ means virtual memory usage
# RSS means real memory usage
$ ps axfwwwww
# informations are not truncated and it supports tree structure
```

- pstree (Process Status TREE): Output the information using tree structure

### 2

- top: Output a list of processes every constant time with refresh
- nohup (NO HangUPs): run shell script file as a format of daemon, redirect stdout to selected file

> daemon: once running, its program continues in background

```bash
$ nohup echo "Bash Coommand"
nohup: ignoring input and appending output to 'nohup.out'
$ cat nohup.out
Bash Command
```

- kill: terminate process by sending selected signal to process

```bash
# INT and TERM help process to be securely terminated
# KILL
$ kill -2 <pid> # kill -INT <pid>
$ kill -15 <pid> # kill -TERM <pid>
# zombie process never die after INT and TERM
$ kill -9 <pid> # kill -KILL <pid>

```

## Network

### 1

- ifconfig (InterFace CONFIGuration): Configuration and Activation/Deactivation of Network interface
- ip: Configuration and Search ip-related information

```bash
$ ifconfig
$ ip address show

$ ifconfig eth0
$ ip address show eth0
$ ip ad sh eth0 
```

- netstat (NETwork STATistics): Output the statistics and connection state of network protocol
- ss (Socket Statistics): Output the statistics and connection state of network socket

```bash
$ netstat -nltpu # TCP and UDP
$ netstat -nltp # exclude UDP
$ netstat -ltp 
# n shows IP or port number instead of defined or domain name
# l means showing sockets currently listening (server always waits for user login by opening certain ports)
# t means tcp, u means udp
$ vi /etc/services 

$ ss -nltpu

$ netstat -tanu
$ ss -tanu
```

### 2

- iptables: tool for packet filtering, used for firewall constraining packet or NAT (Network Address Translation)
  - **firewall initially blocks all inbound connections and opens only necessary ones**
- utw (Uncomplicated FireWall): tool for easy-control of iptables


```bash
$ iptables -nL
# n shows IP or port number instead of defined or domain name
# L means list
## Chain INPUT - Rule appliled to inbound connection (from outside to the server)
## Chain FORWARD - Rule applied to layover connection (via the server)
## Chain OUTPUT - Rule applied to outbound coonection (from the server to outside)
# Rules are applied by top-down order
$ systemctl status iptables

```

- ping: tool for confirming response of ICMP protocol

```bash
$ cat /etc/hosts
127.0.0.1 localhost
...

$ ping -c 5 localhost
PING localhost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.024 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.023 ms
64 bytes from localhost (127.0.0.1): icmp_seq=3 ttl=64 time=0.018 ms
64 bytes from localhost (127.0.0.1): icmp_seq=4 ttl=64 time=0.024 ms
64 bytes from localhost (127.0.0.1): icmp_seq=5 ttl=64 time=0.023 ms

--- localhost ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4103ms
rtt min/avg/max/mdev = 0.018/0.022/0.024/0.002 ms

```

### 3

- wget (World wide web + GET): tools for taking contents from web server
- curl (Client for URLs): tools for sending data using various protocols

```bash
$ wget <file_address>
$ curl -Lkso /dev/null -w "%{http_code}\n" https://gmail.com
200
# L means following link
# k means ignoring identification of http protocol
# s means skipping statistics
# o means selecting output file (/dev/null)
## /dev/null means taking only output and throwing output file

$ curl https://gmail.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="https://mail.google.com/mail/u/0/">here</A>.
</BODY></HTML>
$ curl -Lko /dev/null -w "%{http_code}\n" https://gmail.com
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   230  100   230    0     0    764      0 --:--:-- --:--:-- --:--:--   766
100   370    0   370    0     0    354      0 --:--:--  0:00:01 --:--:--   675
  0     0    0     0    0     0      0      0 --:--:--  0:00:01 --:--:--     0
100   587  100   587    0     0    383      0  0:00:01  0:00:01 --:--:--  573k
100  571k    0  571k    0     0   277k      0 --:--:--  0:00:02 --:--:--  277k
```

- route: tools for printing and changing a route information (routing table) of network

```bash
$ route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         _gateway        0.0.0.0         UG    0      0        0 eno8303
10.178.97.0     0.0.0.0         255.255.255.0   U     0      0        0 eno8303
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker0
172.18.0.0      0.0.0.0         255.255.0.0     U     0      0        0 br-f7b63e7d8fc8
172.19.0.0      0.0.0.0         255.255.0.0     U     0      0        0 br-e35ce6e85ac4
# -n option is identical with netstat
```

## Search

## I/O

## ETC

## Miscellaneous
