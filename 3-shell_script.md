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

```bash
if [condition_1]; then
# following commands are executed if condition_1 is true
elif [condition_2]; then
# following commands are executed if condition_1 is false and condition_2 is true
else
# following commands are executed if both condition_1 and condition_2 are false
fi

$ [a == a] && echo ok || echo ng # single-line if
```

|Number comparison|Description|Char comparison|Description|File comparison|Description|
|---|---|---|---|---|---|
|A -eq B|is A and B identical?|A == B|char A and B are identical?|-d file|does file exist and is it directory?|
|A -ge B|is A greater than or equal to B?|A != B|char A and B are inequal?|-e file|does file exist?|
|A -gt B|is A greater than B?|A < B|byte size of char/string A is less than that of B?|-f file|does file exist and is it file?|
|A -le B|is A less than or equal to B?|A > B|byte size of char/string A is greater than that of B?|-r file|does file exist and is it readable?|
|A -lt B|is A less than B?|-n A|length of char/string A is greater than zero?|-s file|does file exist and is it not empty?|
|A -ne B|is A not equal to B?|-z A|length of char/string A is zero?|-w file|does file exist and is it writeable?|

```bash
#!/bin/bash

LOAD=$(command_bring_load_average)

if [ "${LOAD}" -ge 5]; then
  # command sending warning message
else
  echo "No problem"
fi
```

```bash
[ condition_1 -a condition_2 -a condition_3] # true if condition_{1, 2, 3} are all true
[ condition_1 -o condition_2 -o condition_3] # true if at least one condition is true

```

## Case

```bash
case ${variable_name} in
  pattern_1)
    <command> ;; # executed if pattern_1 is true
  pattern_2)
    <command> ;; # executed if pattern_2 is true
  ...
  *)
    <command> ;; # executed if all upper cases are skipped
esac
```

```bash
#!/bin/bash

# $ ./nginx_ctl.sh a b c
# $1: a, $2: b, $3: c

CMD=$1

case "${CMD}" in
start)
  <command_nginx_start>;;
stop)
  <command_nginx_stop>;;
reload)
  <command_nginx_reload>;;
configtest)
  <command_nginx_configtest>;;
*)
  echo "How to use: ./nginx_ctl.sh {start|stop|reload|configtest}";;
esac
```

## Iteration (Loop)

- for-loop

```bash
for variable in input_data_to_variable
do
  executed command till the end of loop
done

#!/bin/bash
for SVR in cent1 cent2 cent3
do
  echo ${SVR}
done

# $(command) uses when we use the result of command
```

- while-loop

```bash
while condition
do
  executed command as long as the condition is true
done

#!/bin/bash
NUM=1
while [ "${NUM}" -le 3]
do
  echo "cent${NUM}"
  NUM=$((${NUM}+1))
done

while read SVR
do
  echo ${SVR}
done < serverlist
```

## Function

```bash
function <function_name> {
  commands
}

# echo "====="
# echo "df result"
# echo "====="
# df -h
# echo "====="
# echo "====="
# echo "free result"
# echo "====="
# free -m
# echo "====="

#!/bin/bash
function line {
  echo "====="
}

line
echo "df result"
line
df -h
line
echo "free result"
line
free -m
line
```

```bash
# calc
function plus{
  echo "$1 + $2 = "
  echo $[ $1 + $2 ]
  echo
}

function minus{
  echo "$1 - $2 = "
  echo $[ $1 - $2 ]
  echo
}

function multi{
  echo "$1 * $2 = "
  echo $[ $1 * $2 ]
  echo
}

function div{
  echo "$1 / $2 = "
  if [ $2 -eq 0]
  then
    echo "divide-by-zero"
  else
    echo $[ $1 / $2]
  fi
  echo
}

...
# use_calc.sh

#!/bin/bash
source ./calc

plus 30 40
minus 10 3
multi 11 7
div 2 0
div 14 2
```

## Array and Redirect

- array: contains number or character. rarely used because it makes the problem complicated.
  - instead, use file

```bash
$ test_array = (one two three 4 5 6 7)
$ echo ${test_array}
one
$ echo ${test_array[0]} # default index is zero
one
$ echo ${test_array[*]}
one two three 4 5 6 7
$ for i in $(seq 0 7); do echo ${test_array[${i}]}; done
one
two
three
4
5
6
7
```

- redirect: by changing the direction of standard stream (input/output/error), help to use text file in shell script. 

```bash
$ ls -al txt1 txt2 txt3 txt4
ls: cannot access 'txt4': No such file or directory
... txt1
... txt2
... txt3
$ ls -al txt1 txt2 txt3 txt4 1> ok 2> ng # stdout 1, stderr 2
$ cat ok
... txt1 
... txt2 
... txt3
$ cat ng
ls: cannot access 'txt4': No such file or directory

```

```bash
system_report.sh
#!/bin/bash

# create report file
touch report
# initialize report file
cp -f /dev/null report

# > (overwrite) >> (append)
{
  echo "===== df ====="
  df -h
  echo
  echo "===== pstree ====="
  pstree
  echo
  echo "===== free ====="
  free -m
  echo
  echo "===== uptime ====="
  uptime
  echo
}>> report # OR |tee -a report (print and file output)

# if send an email
cat report | mail -s "[report] cent system info" email

$ ./system_report.sh
$ cat report

```

## Interactive

```bash
read_test.sh

#!/bin/bash
read -sp "Type any characters. : " ANY
# s means secret mode (Input is not printed)
echo "What you type is ${ANY}."

$ chmod 700 read_test.sh
$ ./read_test.sh
Type any characters. : # type aaaaa 
What you type is aaaaa
```

```bash
#!/bin/bash

# up-down game
# 1. set target number GOAL between 1 and 100
# 2. type input number
# 3-1) if input > GOAL
# 3-2) elif input < GOAL
# 3-3) else

GOAL=$(($RANDOM%100 + 1))
CNT=0
while true
do
  read -p "Type the number among 1~100. : " NUM
  CNT=$((${CNT} + 1))
  if [ ${NUM} -gt ${GOAL} ]
  then
    echo "Your number is greater than GOAL."
  elif [ ${NUM} -lt ${GOAL} ]
  then
    echo "Your number is less than GOAL."
  elif [ ${NUM} -eq ${GOAL}]
  then
    echo "Congrats! You find the number in count ${CNT}"
    exit 0 # code 0 means success (1 means error)
  fi
  
done
```

## Menu

### Without 'dialog'

```bash
menu.sh
#!/bin/bash

function menu {
clear
cat << EOF
  ============= Menu ================
  1. Load average
  2. disk state
  3. memory state
  4. process tree
  5. ssh connection
  6. exit menu
  ===================================

EOF

read -p "Select menu number. : " SELECT
}

function press_key {
  echo
  read -n1 -rsp "Press any key to continue..."
  echo
  echo
}

while true
do
  menu
  case ${SELECT} in
    1)
      clear
      echo "Load average"
      uptime
      press_key
      ;;
    2)
      clear
      echo "Disk state"
      df -h
      press_key
      ;;
    3)
      clear
      echo "Memory state"
      free -m
      press_key
      ;;
    4)
      clear
      echo "Process tree"
      pstree
      press_key
      ;;
    5)
      clear
      read -p "Type server name you want to connect: " SVR
      sleep 1
      echo "Connecting to ${SVR}..."
      ssh ${SVR}
      ;;
    6)
      exit 0
      ;;
    *)
      echo "You type a wrong number."
      press_key
      ;;

  esac
done

$ chmod 700 ./menu.sh
$ ./menu.sh
```

### With `dialog --option`
GUI (Graphic User-Interface)

|Option|Description|Option|Description|
|---|---|---|---|
|calendar|Date Choose|gauge|Progress bar|
|checklist|Multiple choose|infobox|Message|
|radiolist|Single Choose|menu|Select list|
|fselect|File selection window|msgbox|Message and show OK button|
|passwordbox|Hide Input text|yesno|Message and show Yes, No buttons|

```bash
#!/bin/bash

# identical with menu.sh but a few fixes are needed.

function menu {
  dialog --title " GUI MENU " \
        --radiolist " Choose Menu. " 20 35 6 \ # width, height, num_of_menu
        1 "Load average" off \ # menu_number, menu_name, choose_state_default
        2 "Disk state" off \
        3 "Memory state" off \
        4 "Process tree" off \
        5 "Connect to server" off \
        6 "Exit" off \
        2>./select # menu_number is considered as stderr and it goes to ./select file
}

...

while true
do
  menu
  SELECT=${cat select} # read menu_number from select file

```