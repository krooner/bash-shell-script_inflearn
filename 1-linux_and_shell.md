# Linux and Shell

## Type of Linux Distributed Versions
- Debian: Ubuntu, Mint, Kali
- Slackware: OpenSUSE
- Gentoo
- RedHat: Pedora, CentOS

> MacOS from Unix BSD (Berkeley Software Distribution)

## Type of Shell

|Name|Feature|Reference File|Program|Prompt|
|---|---|---|---|---|
|sh (Bourne Shell)|First Unix Shell|~/.profile|/bin/sh|$ (General-user) <br> # (root-user)|
|csh (C Shell)|Distributed for Unix BSD|~/.cshrc|/bin/csh|% (G) <br> # (root)|
|ksh (Korn Shell)|Faster than csh and compatible with script written by sh|~/.ksh|/bin/ksh|$ (G) # (root)|
|bash (Bourne-Again Shell)|Linux Default Shell in GNU Project|/etc/profile<br>/etc/bashrc<br>~/.bash_profile<br>~/.profile<br>~/.bashrc|/bin/bash|$ (G) # (root)|

## GUI and CLI
- GUI (Graphic User Interface) uses X-Window (X11)
- CLI (Command-Line Interface) displays only using text

## Useful Bash command line shortcut
- `Tab`: Command autocomplete
- `^c`: Terminate running process by sending interrupt signal
- `^a`: Move cursor to the *first point* of line
- `^e`: Move cursor to the *last point* of line
- `^r`: Search history

```bash
# `cd`: Change Directory
# `pwd`: Print Working Directory
$ num=0;while true;do ((num+=1));echo ${num};sleep 1;done
1
2
3
^C # type ^C
...
$ # type ^C
(reverse-i-search)`num': num=0;while true;do ((num+=1));echo ${num};sleep 1;done 

```
