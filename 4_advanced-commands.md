# Advanced Commands

## 1. Text processing

### SED
- get text input stream and control output
- `sed -option string_processing_script filename`
 
|Option|Description|
|---|---|
|-e|a single processing command to handle input. multiple define and process.|
|-f file|indicate a file containing pre-defined command|
|-n|result of each command waits for print command|
|-r|use expanded regular expression|

|Command|Name|Description|
|---|---|---|
|[text_line_or_boundary]/p|print|-|
|[text_line_or_boundary]/d|delete|-|
|s/<source_pattern>/<dest_pattern>/|substitute|replace source_pattern with dest_pattern|
|[text_line_or_boundary]/s/<source_pattern>/<dest_pattern>/|substitute|replace source_pattern with dest_pattern under defined boundary|
|[text_line_or_boundary]/y/<source_pattern>/<dest_pattern>/|transform|replace source_pattern with dest_pattern under defined boundary (num_char should be identical)|
|<string_processing_script>/g|global|all source_pattern in string_processing_script with dest_pattern|

```bash
$ echo "This is test sentence." | sed 's/test/TEST/'
This is TEST sentence.
$ cat data.txt
1. This is test sentence.
2. This is test sentence.
3. This is test sentence.
...
7. This is test sentence.
$ sed 's/test/TEST' data.txt # original file is not changed
1. This is TEST sentence.
2. This is TEST sentence.
3. This is TEST sentence.
...
7. This is TEST sentence.
$ sed '3,5s/test/TEST' data.txt # only line 3 and 5 are applied
1. This is test sentence.
2. This is test sentence.
3. This is TEST sentence.
4. This is test sentence.
5. This is TEST sentence.
...
7. This is test sentence.
$ sed '3,5d' data.txt # only line 3 and 5 are removed
1. This is test sentence.
2. This is test sentence.
4. This is test sentence.
...
7. This is test sentence.
$ sed 's/test/TEST/w data2.txt' data.txt # data2.txt contains processed data
$ sed -e 's/test/TEST; s/sentence/SENTENCE' data.txt
1. This is TEST SENTENCE.
2. This is TEST SENTENCE.
3. This is TEST SENTENCE.
...
7. This is TEST SENTENCE.
$ sed -n 's/test/TEST' data.txt # output is not printed
$ sed -n 's/test/TEST/p' data.txt # option p means print
1. This is TEST sentence.
2. This is TEST sentence.
3. This is TEST sentence.
...
7. This is TEST sentence.

$ cat two.sed
s/test/TEST
s/sentence/SENTENCE
$ sed -f two.sed data.txt
1. This is TEST SENTENCE.
2. This is TEST SENTENCE.
3. This is TEST SENTENCE.
...
7. This is TEST SENTENCE.
```

## 2. Regular Expression
Express text pattern as special characters and symbols

|meta-char|meaning|matched strings|
|---|---|---|
|A*|greater than or equal to zero char right after A|A Aa Abc A13cg ...|
|A?|a single char right after A|Aa Ab Ac A1 Az ...|
|^A|string starts with A|Abcde Avoid Attack ...|
|A$|string ends with A|A ba cdgdA abcdefA ...|
|A|B|contain A or B|c[A|B]f : cAf cBf ...|
|[a-z]|alphabet from a to z|a b c d e ...|

```bash
$ ls -1 a* # option 1 means printing only filename
a2
aaa
$ ls -1 | grep ^a
a2
aaa
$ find . -type f -iname "t*" # file starts with t (iname recognizes regex)
./testfile
./two.sed
./txt
./tzt
./tpt
./tct
...
$ find . -type d -iname "t*" # directory starts with t
./tmp
$ find . -iname "t[x|z]t"
./txt
./tzt
$ ls -al txt[0-9]
... txt1
... txt2
... txt3
$ ls -al txt[0-9][0-9]
... txt10
... txt97
...
$ sed -n '/^3/p' data.txt # line starts with 3
3. This is test sentence.
$ sed -n '/c$/p' txt1 # line ends with c
ccc
```


## 3. AWK
- including features of SED, enable programming to control necessary data using variable, function, and operator.
- CUT: extract string field from file. it can be replaced by AWK


|Option|Description|
|---|---|
|-F <field_delimiter>|select delimiter to discriminate field|
|-f <file_name>|select file name which awk reads|
|-<variable>=<value>|selct variable used by awk|

```bash
$ cat txt13
aaa  111  zzz
bbb  222  xxx
ccc  333  yyy
ddd  444  qqq
eee  555  ddd
# tab-space is delimiter

$ awk '{print $1}' txt13
aaa
bbb
ccc
ddd
eee
$ awk '{print $2}' txt13
zzz
xxx
yyy
qqq
ddd

$ awk '{print $1}' txt13 | grep ccc
ccc
$ awk '/ccc/ {print $1}' txt13 # same result
ccc

$ grep 111 txt13 | awk '{print $3}'
zzz
$ awk '/111/ {print $3}' txt13
zzz

$cat colon_txt13
aaa:111:zzz
bbb:222:xxx
ccc:333:yyy
ddd:444:qqq
eee:555:ddd
$ awk -F: '{print $1}' colon_txt13
aaa
bbb
ccc
ddd
eee

$ vi /etc/passwd
$ awk -F: '{print $1} /etc/passwd # prints all username in /etc/passwd
...
$ uptime
14:51  up  5:44, 2 users, load averages: 3.13 2.82 2.45
$ uptime | awk '{print $8, $9, $10}'
3.13 2.82 2.45

$ cat df_chk.awk
{
  gsub(/%/,"")
  PER=0; MNT=""
  if (NF==6) PER=$5
  if (PER>30) MNT=$6
  printf "$%%\t%s\n",PER,MNT
}
$ df | awk -f df_chk.awk | grep \/
35%  /

```
