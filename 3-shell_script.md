# Shell script

## Consecutive execution of multiple commands
- pipeline (|): standard stream from left-side result of command is used as input of right-side command

```bash
$ cat testfile.txt | grep 777 # check whether testfile.txt contains 777
$ grep 777 testfile.txt # same result

$ df -h | grep sda1 # check the storage of root partition
$ ssh <remote_server_ip> "df -h | grep sda1" # connect to remove server and check its root partition storage
```

- semi-colon (;): execute right-side command after finishing left-side command

```bash
$ ssh <remote_server_ip> "ls -al /var/log ; df -h" # check the log directory file list and disk statistics of remote server
```

- AND (&&): if left-side command result is true, then execute right-side command. otherwise, don't execute right-side one.
- OR (||): if left-side command result is true, then don't execute right-side command. otherwise, execute right-side one.

```bash
$ mkdir tmp; touch tmp/tmp.txt; echo "tmp" > tmp/tmp.txt; cat tmp/tmp.txt
$ ssh cent3 "ps -ef|grep nfs"
$ ssh cent3 "ps -ef|grep nfs|grep -v grep|wc -l"
$ [-e /var/log/messages] && tail -n 15 /var/log/messages
$ mount | column -t 
```

## CLI (Command-line interface) Editors
- nano
  - ^G: manual
  - ^O: save
  - ^Y: previous page
  - ^V: next page
  - ^K: cut
  - ^U: cancel cut
  - ^W: find
  - ^X: exit
- vim
  - normal mode: edit or move cursor - h (left), j (down), k (up), l (right)  
  - insert mode: a (move cursor to next position), i (current cursor position), o (move cursor to newline)
  - visual mode (v, V, ^v): select boundary and copy (y), cut (x), delete (d), paste (p)
  - ex mode: execute command or search
    - save and terminate w/ colon(:): w(save), q(terminate), wq(save and terminate), w! (forcely save), q! (forcely quit)
    - execute bash command w/ colon(:): !{command}
    - search w/ slash(/) in current document: /{target_string}

## Writing shell script
  
```bash
### helloworld.sh
#!/bin/bash # #! is called 'shabang'

# in case of python, use #!/usr/bin/env python
# in case of perl, use #!/usr/bin/env perl

echo "Hello World!?"

TXT="Hello world"

echo "${TXT}"
```

```bash
$ bash helloworld.sh
Hello World!?
Hello world
```

## If

## Case

## Iteration

## Function

## Array and Redirect
