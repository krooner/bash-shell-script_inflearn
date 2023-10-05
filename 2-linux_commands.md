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

- find: search file using selected file name or regular expression

```bash
$ find <option> <path_start_to_look_for> <expression>
$ ls 
nohup.out
$ find ./ -name nohup.* # * asterisk matches with charaters 
nohup.out
$ find ./ -name no?up.out # ? question mark matches with a single character
nohup.out
```

- which: tool for finding commands in directory registered in environment variable PATH

```bash
$ echo ${PATH}
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
$ which ls
/usr/bin/ls
$ which df
/usr/bin/df
$ which find
/usr/bin/find
$ which ps
/usr/bin/ps
```

- grep (Global / Regular Expression / Print): tool for text search, output line matched with regex in file or stdin 

```bash
$ grep <option> <string_to_find> <file_name>
# string_to_find is case-sensitive: "Error" and "error" is different string
$ grep -i "error" /var/log/*
# -i option means case-insensitive
$ grep -ir "error" /var/log/*
# -r option means recursive: includes directory
# -c option means count: how many target_string is included in target_file
```

- history: output/manipulate list of command execution

```bash
$ tail .bash_history
ping -c 5 localhost
curl -Lkso /dev/null -w "%{http_code}\n" https://gmail.com
curl https://gmail.com
curl -L https://gmail.com
curl -Lko https://gmail.com
curl -Lko /dev/null -w "%{http_code}\n" https://gmail.com
route
route -n
exit
ls
$ history
    1  exit
    2  which bash
    3  sh
    4  ls
    5  vim
...
  177  man grep
  178  ls -al .bash_history 
  179  tail -n 10 .bash_history 
  180  tail .bash_history
  181  history
```

## I/O 

- redirection: send standard stream (stdin, stdout, stderr) using inequality sign
- echo: tool for string output

```bash
$ <command> <redirection_symbol> <file_name>
$ echo b > a # overwrite string to file (possible error)
$ cat a
b
$ echo zzz >> a # append string
$ cat a
b
zzz
$ echo kkk >| a # forcely create file (no error)
$ cat a
kkk
```

- chmod (CHange MODe): tool for changing access permission of file or directory

```bash
$ ls -al
drwxr-xr-x   4 kso.kim  staff      128 Sep 26 10:45 opencv
lrwxr-xr-x   1 kso.kim  staff       12 Oct  4 14:50 symbolic-link.txt -> hardlink.txt
-rw-r--r--   1 kso.kim  staff       12 Oct  4 14:48 testfile
-rw-r--r--   1 kso.kim  staff       11 Oct  4 15:00 testfile2

# first rwx is permission for file owner
# second rwx is permission for user's group
# last rwx is permission for others (neither file owner nor user's group)

$ chmod <permission_info> <file_name>
```
  
- chown (CHange the OWNer of a file): tool for changing ownership of file

```bash
$ chown <user_name>:<group_name> <file_or_directory_name>
```
  
- sudo (SUperuser DO): tool for executing commmand or program using security permission of root user
- who: output list of currently logged in user in system

```bash
$ w
11:17  up 1 day,  2:09, 2 users, load averages: 1.29 1.60 1.76
USER     TTY      FROM              LOGIN@  IDLE WHAT
kso.kim  console  -                Wed09   25:37 -
kso.kim  s000     -                Wed10       - w
```
  
## ETC

- date: output current day and time

```bash
$ date
Thu Oct  5 05:52:35 UTC 2023
$ date -d '-1 day'
Wed Oct  4 05:53:07 UTC 2023
$ date -d '1 day ago'
Wed Oct  4 05:53:38 UTC 2023
$ date -d '1 day'
Fri Oct  6 05:53:54 UTC 2023

$ date '+%Y-%m-%d %H:%M:%S'
2023-10-05 05:55:56
$ date '+%Y%m%d'
20231005
```

- seq (SEQuence): tool for printing number with defined order

```bash
$ seq 1 5
1
2
3
4
5
$ seq -w 1 10
01
02
03
04
05
06
07
08
09
10
$ seq -f %03g 1 10
001
002
003
004
005
006
007
008
009
010
```

- more: tool for printing file content in screen
- watch: tool for re-executing command every defined time and printing the result on the screen
- crontab: tool for printing or editing the contents of job scheduler in linux

## Miscellaneous
