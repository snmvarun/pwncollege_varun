# Data Manipulation
## Translating characters 
One of the purposes of piping data is to modify it. Many Linux commands will help you modify data in really cool ways. One of these is tr, which translates characters it receives over standard input and prints them to standard output.

In its most basic usage, tr translates the character provided in its first argument to the character provided in its second argument:
```bash
hacker@dojo:~$ echo OWN | tr O P
PWN
hacker@dojo:~$
```
It can also handle multiple characters, with the characters in different positions of the first argument replaced with associated characters in the second argument.
```bash
hacker@dojo:~$ echo PWM.COLLAGE | tr MA NE
PWN.COLLEGE
hacker@dojo:~$
```
Now, you try it! In this level, /challenge/run will print the flag but will swap the casing of all characters (e.g., A will become a and vice-versa). Can you undo it with tr and get the flag?

### Solve 
**Flag: pwn.college{wmOggH75PzMDB94FtG8zeL6K0A6.01MxEzNxwCN1kjNzEzW}**

```bash
hacker@data~translating-characters:~$ /challenge/run
Your case-swapped flag:
PWN.COLLEGE{Opx5ExZOldiG8vw_IHyNuLpaFNZ.01mXeZnXWYnWaZnZeZw}
hacker@data~translating-characters:~$ echo PWN.COLLEGE{Opx5ExZOldiG8vw_IHyNuLpaFNZ.01mXeZnXWYnWaZnZeZw} | tr abcdefghijklmnopqrstuvwx
yzABCDEFGHIJKLMNOPQRSTUVWXYZ ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz
pwn.college{oPX5eXzoLDIg8VW_ihYnUlPAfnz.01MxEzNxwyNwAzNzEzW}
```
I ran /challenge/run to get the case swapped flag which I changed the flag's case of letters by translating small letters to big ones and big letters to small ones.

### New Learnings
How to use the tr command which translates characters it receives over standard input and prints them to standard output.

## Deleting characters 
tr can also translate characters to nothing (i.e., delete them). This is done via a -d flag and an argument of what characters to delete:
```bash
hacker@dojo:~$ echo PAWN | tr -d A
PWN
hacker@dojo:~$
```
Pretty simple! Now you give it a try. I'll intersperse some decoy characters (specifically: ^ and %) among the flag characters. Use tr -d to remove them!
### Solve 
**Flag: pwn.college{408zTiGqx-GbHhtIvBSQFJqq4Y_.0FNxEzNxwyNwAzNzEzW}**

```bash
hacker@data~deleting-characters:~$ /challenge/run
Your character-stuffed flag:
p^%w^n%.^c^%o^%l^%l%eg^%e{^%4%0%8z^%T^i^G%qx^%-^Gb^H^ht^%I^v^B%SQ^%FJ^q%q^%4^Y^%_%.^%0^F^%N^%x^E%z^N^%x^w%y^Nw^%A^z^%N^%z^E%z^W^%}^%^%
hacker@data~deleting-characters:~$ echo p^%w^n%.^c^%o^%l^%l%eg^%e{^%4%0%8z^%T^i^G%qx^%-^Gb^H^ht^%I^v^B%SQ^%FJ^q%q^%4^Y^%_%.^%0^F^%N^%x^E%z^N^%x^w%y^Nw^%A^z^%N^%z^E%z^W^%}^%^% | tr -d ^%
pwn.college{408zTiGqx-GbHhtIvBSQFJqq4Y_.0FNxEzNxwyNwAzNzEzW}
```
I ran /challenge/run to get the character stuffed flag, with that I noticed that the flag contents are interuppted by two characters % and ^, so i used the tr -d command to delete the interupting characters and obtain the flag  

### New Learnings
How to delete characters using the -d argument in the tr command

## Deleting newlines 
A common class of characters to remove is a line separator. This happens when you have a stream of data that you want to turn into a single line for further processing. You can specify newlines almost like any other character, by escaping them:
```bash
hacker@dojo:~$ echo "hello_world!" | tr _ "\n"
hello
world!
hacker@dojo:~$
```
Here, the backslash (\) signifies that the character that follows it is a standin for a character that's hard to input into the shell normally. The newline, of course, is hard to input because when you typically hit Enter, you'll run the command itself. \n is a standin for this newline, and it must be in quotes to prevent the shell interpreter itself from trying to interpret it and pass it to tr instead.

Now, let's combine this with deletion. In this challenge, we'll inject a bunch of newlines into the flag. Delete them with tr's -d flag and the escaped newline specification!

Fun fact! Want to actually replace a backslash (\) character? Because \ is the escape character, you gotta escape it! \\ will be treated as a backslash by tr. This isn't relevant to this challenge, but is a fun fact nonetheless!

### Solve 
**Flag: pwn.college{IQSMgvZzqAAj2k-v1NFuQlvXULr.0VNxEzNxwyNwAzNzEzW}**

```bash
hacker@data~deleting-newlines:~$ /challenge/run
Your line-split flag:
pwn
.
c
o
l
l
e
g
e
{
IQS
MgvZz
qA
Aj
2k-
v1
NF
uQ
l
v
X
U
L
r
.
0V
N
xE
zN
x
wyNwA
zN
z
Ez
W}

hacker@data~deleting-newlines:~$ echo "pwn
.
c
o
l
l
e
g
e
{
IQS
MgvZz
qA
Aj
2k-
v1
NF
uQ
l
v
X
U
L
r
.
0V
N
xE
zN
x
wyNwA
zN
z
W}" | tr -d "\n"
pwn.college{IQSMgvZzqAAj2k-v1NFuQlvXULr.0VNxEzNxwyNwAzNzEzW}
```

### New Learnings


## Extracting the first lines with head
In your Linux journey, you'll experience situations where you need to grab just the early output of very verbose programs. For this, you'll reach for head! The head command is used to display the first few lines of its input:
```bash
hacker@dojo:~$ cat /something/very/long | head
this
is
just
the
first
ten
lines
of
the
file
hacker@dojo:~$
```
By default, it shows the first 10 lines, but you can control this with the -n option:
```bash
hacker@dojo:~$ cat /something/very/long | head -n 2
this
is
hacker@dojo:~$
```
This challenge's /challenge/pwn outputs a bunch of data, and you'll need to pipe it through head to grab just the first 7 lines and then pipe them onwards to /challenge/college, which will give you the flag if you do this right! Your solution will be a long composite command with two pipes connecting three commands. Good luck!

### Solve 
**Flag: pwn.college{sS-uN9AtZEGkc71TS2BcFzt_9dw.0lNxEzNxwyNwAzNzEzW}**

```bash
hacker@data~extracting-the-first-lines-with-head:~$ /challenge/college < <( /challenge/pwn | head -n 7 )
Congratulations, you piped the right codes!
pwn.college{sS-uN9AtZEGkc71TS2BcFzt_9dw.0lNxEzNxwyNwAzNzEzW}
```
I basically first piped the first 7 lines of /challenge/pwn using the head -n 7 command and piped it through to /challenge/college

### New Learnings
How to use the head command and how it can be used to print a number of lines of the output

## Extracting specific sections of text
Sometimes, you want to grab specific columns of data, such as the first column, the third column, or the 42nd column. For this, there's the cut command.

For example, imagine that you have the following data file:
```bash
hacker@dojo:~$ cat scores.txt
hacker 78 99 67
root 92 43 89
hacker@dojo:~$
```
You could use cut to extract specific columns:
```bash
hacker@dojo:~$ cut -d " " -f 1 scores.txt
hacker
root
hacker@dojo:~$ cut -d " " -f 2 scores.txt
78
92
hacker@dojo:~$ cut -d " " -f 3 scores.txt
99
43
hacker@dojo:~$
```
The -d argument specifies the column delimiter (how columns are separated). In this case, it's a space character. Of course, it has to be in quotes here so that the shell knows that the space is an argument rather than a space separating other arguments! The -f argument specifies the field number (which column to extract).

In this challenge, the /challenge/run program will give you a bunch of lines with random numbers and single characters (characters of the flag) as columns. Use cut to extract the flag characters, then pipe them to tr -d "\n" (like the previous level!) to join them together into a single line. Your solution will look something like /challenge/run | cut ??? | tr ???, with the ??? filled out.

### Solve 
**Flag: pwn.college{MaKVdE1CZSxQCm5p7aygopsiPTv.01NxEzNxwyNwAzNzEzW}**

```bash
hacker@data~extracting-specific-sections-of-text:~$ /challenge/run
24600 p
28125 w
24625 n
16815 .
1347 c
1642 o
9477 l
3282 l
25673 e
32324 g
14586 e
21904 {
32526 M
3921 a
10953 K
6488 V
25478 d
3136 E
18499 1
14240 C
5313 Z
4519 S
27911 x
30123 Q
11881 C
28832 m
16829 5
4123 p
1781 7
30798 a
20588 y
30537 g
5320 o
637 p
26445 s
29899 i
21214 P
32371 T
20834 v
6079 .
2749 0
2232 1
5839 N
2892 x
27151 E
8802 z
1726 N
17403 x
20856 w
18082 y
19577 N
15663 w
29250 A
24316 z
7390 N
19273 z
12805 E
26705 z
17744 W
2309 }
hacker@data~extracting-specific-sections-of-text:~$ /challenge/run | cut -d " " -f 2 | tr -d "\n"
pwn.college{MaKVdE1CZSxQCm5p7aygopsiPTv.01NxEzNxwyNwAzNzEzW}
```
I first ran /challenge/run to check out its contents to find the flag in column two, then i ran the command /challenge/run while piping it to cut down and only output the 2nd column contents, these contents would now be seperated by a new line therefore deleting the new line using the tr -d "\n" gives me the flag. 

### New Learnings
How to extract columns of content from a dataset using the cut command

##

### Solve 
**Flag: pwn.college{ogEoS_x8DRxXX8iczPuf1Pi1u7q.0FM0MDOxwyNwAzNzEzW}**

```bash
hacker@data~sorting-data:~$ sort /challenge/flags.txt -r
pwn.college{ogEoS_x8DRxXX8iczPuf1Pi1u7q.0FM0MDOxwyNwAzNzEzW}
pwn.college{ogEoS_x8DRxXX8iczPuf1Pi1u7q.0FM0MDOwwyNwAzNzEzW}
pwn.college{ogEoS_x8DRxXX8iczPuf1Ph1u7q.0FM0MDNxwyNwAzNzEzW}
pwn.college{ogEoS_x8DRxXX8iczPuf0Pi1u7q.0FM0MDOxwyNwAzMzEzW}
pwn.college{ogEoS_x8DRxXX8iczOuf1Pi1u7q.0FM0MDOxwyNwAzNzEzW}
pwn.college{ogEoS_x8DRxXX8ibzPue1Pi1u7q.0FM0MDOxwyNwAzNzEzW}
pwn.college{ogEoS_x8DRxXX8ibzOuf1Pi1u7q.0FM0MCOxwxNwAyNzEzW}
pwn.college{ogEoS_x8DRxXX7iczPuf1Pi1u7q.0FL0MDOxwyNwAzNzEzW}
pwn.college{ogEoS_x8DRxXW8icyPtf1Pi1u7q.0FL0MDOxwyNwAzNzEzW}
pwn.college{ogEoS_x8DRwXX8iczPuf0Pi1u6q.0FM0LCOxwyMwAzNyDzW}
pwn.college{ogEoS_w8DRxXX8iczPuf1Pi1u7q.0EM0MDOxwxNwAzNzEzW}
pwn.college{ogEoR_x8DRxXX8iczPuf1Pi1u7q.0FM0MDOxwyNwAzNzEzV}
pwn.college{ogEoR_x8DRxXX7ibzPte1Oi1u7q.0FM0MDOxwyNvAzMzEzV}
pwn.college{ogEnS_x8DRxXX8iczPuf1Pi1u7q.0FM0MDOxwyNwAzNzEzV}
pwn.college{ogEnR_x7CRxXX7iczPuf1Ph1u7q.0FL0MDOxvyNwAzNzDzW}
pwn.college{ofEoS_x8DRxXX8iczPuf1Oi1u7q.0FM0LDOxwyNwAzNzEzW}
pwn.college{ofEoS_x8DRxXX8iczPuf0Pi1t6q.0FM0LDOxwyNwAzNzEzW}
pwn.college{ofEoS_x8DRxXX8iczPtf1Pi1u7q.0FM0LDOxwyNvAzNzEyW}
pwn.college{ngEoS_x8DRxXX8iczPuf1Pi1u7q.0FM0MDOxwyNwAzNzEzW}
pwn.college{ngEoS_x8DRxXX8iczPuf1Ph1t7q.0EM0MDOxvyNwAyNzEzW}
pwn.college{ngEoS_w8DRxXX8iczPuf1Pi1u7q.0FM0LDOxwyNwAzNzEzW}
pwn.college{ngEoR_x8CRxWX8iczPuf1Pi1u7q.0FM0MDOwwxNvAzNzEzW}
pwn.college{ngEoR_w8DRxXX8icyPuf1Pi1u7q.0FM0MDOxwxNwAzNzDzW}
pwn.college{ngDoS_x8DRwWW8hcyOte0Pi1u6q.0EM0LDNxwyNwAzMzDzV}
pwn.college{nfEoS_x8CRwWW8hczPte0Pi0u7q.0FM0MDOxvyMwAzNyEyV}
pwn.collegd{ogEoR_w8DQxXX8iczOuf1Oi1t7q.0EL0MDOwvyMwAzNzEzW}
pwn.collefe{ofEoS_w8CRxXX8iczOue1Pi0u6p.0FM0LCOxwyNwAzNzEzW}
pwn.collefe{ngEoS_x8DRxXX8iczOuf1Pi1u7q.0FL0MDOxwyNwAzNzEzV}
pwn.collefd{ngDoS_w8DRxXX8icyPuf1Pi1u7q.0FM0MDOxwyMwAyNyEzW}
pwn.colldge{ofEnS_x8DRxXW7iczOtf1Ph1t7p.0FL0MDNwwxNwAzMyEyW}
pwn.colldge{ofEnS_x7DRxXW8ibyOtf0Pi1t7q.0FM0MDNwwyNwAzMyEzV}
pwn.colkege{ogEoS_x8DRxXX8iczPuf1Pi1u7q.0FM0MDOxwyMvAyMyEzW}
pwn.colkege{ogEoS_w7DRxXX8icyPuf0Pi1u7q.0EM0MDOxwyNwAyNzEzW}
pwn.colkefe{ogEoS_w8DRxXX7iczPuf1Pi1u7p.0FM0MCOxwyNwAzNzDzW}
pwn.colkefd{ogDnS_x8CQxXX8iczPuf1Ph0u7q.0FM0LCNxwxMvAzMzEzW}
pwn.colkdge{ogEoS_x8DRxXX7hcyPuf1Pi1t7q.0EM0MCOwwyNwAzMyEyW}
pwn.coklege{ogEoS_x8CQxXW8hczOuf1Pi1u7q.0EM0LDOwwyNwAzNzEzW}
pwn.coklegd{ogEoS_x8DRxXW8iczPuf1Pi1u7q.0FM0MDOxvyNwAzMzEzW}
pwn.cnllege{ogEoS_x8DRwXX8iczPuf1Pi1u7q.0FM0MDOxwyNwAzMzEzW}
pwn.cnllegd{ngEoS_w8DRwWX8iczPuf1Ph1u7q.0FL0MDNxvyNwAzNzEzW}
pwn.cnllefe{ngEoR_x8CRxWX7ibyOte1Ph1u7q.0FM0MDNwwxNvAzMzEzV}
pwn.cnklege{ofDoS_w7CQwWW8ibyOuf1Ph1u7p.0EM0MDOxwyNvAzNzEzV}
pwn.cnkldgd{ngEoS_x8DRxWX8iczPuf1Pi1u7p.0FL0MDOxvyNwAzNzEyW}
pwn.bollege{nfEoS_x8DRxXX8ibzOuf1Pi1u6q.0FM0MDNxvyNwAzNzEzW}
pwn.bolkefe{nfDoS_x8DRxWX8iczPuf1Pi1t7p.0FM0MDOxvyNwAzNyDzW}
pwn.bolkdfe{ngEoS_x8DQxXX8iczPuf0Oi0u7q.0FL0MCOxvyMwAzMzEyW}
pwn.bnllege{ofDoS_x8DRxWX8ibzPuf1Oi0u7q.0FM0MDOwwyNwAzNyEzW}
pwn.bnllefd{nfDoS_x8CRwWW8hbyPtf0Pi1u7p.0FL0MDOxwyNvAzNzDzW}
pwn.bnlldge{ofEnS_x8DQxWX8ibyOuf0Oi0u7p.0EM0MDNxvxMwAzMzEyV}
pwn.bnklefd{ngEoS_w8DRwXX7ibzPuf1Pi1u7q.0EM0MDOwwyNwAyMzEzW}
pwm.college{ogEoS_x7CQxWX8ibyOtf0Pi1t6q.0FL0MDNwwyNvAzNzEzW}
pwm.college{ofEoS_x8CRxXX8hczPuf1Oi0u7q.0FM0MDOxwxNwAyNzDyV}
pwm.collefe{ogEnS_x8CRxXX7iczPue1Pi1u7q.0FM0MDOxwxNwAyNzEzV}
pwm.colldfe{ogEnR_x8DRxXW7iczOuf1Pi1t7p.0FL0LCOxvyMvAyNzEzV}
pwm.cokldfe{ogEoS_x8DQxWW8hbyPuf1Oi1u6p.0FL0MDOwvyMwAyNzDyW}
pwm.cnlkdfe{ofEoS_w8CQxXW7icyPuf1Pi0t6p.0EL0MDNxwyNvAzNzEyW}
pwm.bolkege{ngEoR_x8CQxXW8hbyPtf1Pi1t7q.0FM0MCNxwxNwAzNyEzV}
pwm.bnllegd{ogDoS_x8DRxXX8icyOuf1Pi1u7p.0FM0MDOxwyNwAzMzEzW}
pwm.bnlldge{ofDnR_x8DQxXW8icyOue1Pi1t7q.0FM0MDOxwyMvAzNzEzW}
pvn.college{ogEoS_x8DRxXX8iczPue1Pi1u7q.0FL0LDOxwyNwAyNzEyW}
pvn.college{ogEoS_x8DRxXX8iczPue1Ph1u7p.0FM0MDOxwyNwAzNzEzW}
pvn.collegd{ofEoS_x8CRwXW8iczPuf1Pi1t7q.0EM0MCOxvyMvAzMyDyW}
pvn.collefe{ngEoS_x8DRxXX8iczPuf1Ph1u7q.0FL0MCOxvyNwAzNyEzW}
pvn.colkefe{ogEnS_x7CRxWX8icyOuf1Pi0t7q.0FM0MDOwvyNvAzMzEyW}
pvn.coklegd{ogEoS_w8DRwWX8iczPuf1Pi0u7q.0FM0MCOxwyMwAzNyEzW}
pvn.cokkegd{ogDnR_x7CQxXX7ibzOuf1Pi0t6q.0EM0MDOxwyNwAyMyEzW}
pvn.bnlldge{ngEoS_x8CRwXX8iczPuf1Ph1u7q.0FM0MDOxwyNwAzNzEzW}
pvn.bnkkefd{ogDoS_x8DQxWX8ibyPtf1Pi0u6q.0EL0MDOxwxNwAzNzDyW}
pvm.cokkege{ogEoR_x7CRxXX8iczPuf1Pi1u7p.0EM0MCNxwxMvAyMzEzV}
pvm.cnllefd{ofEnS_x8DRxXW8iczPtf1Pi1u6q.0FL0MDOxwyMwAzNzEzW}
pvm.cnkkegd{ogEoR_x7DRwWW7iczOue1Pi1t7q.0FM0MDNwwyMvAyMzEzW}
pvm.cnkkegd{ofDnR_x8DRxXX8ibzPuf1Pi0t7q.0FL0LDNwwyMvAzNzEzW}
pvm.bollege{ofEnS_x7DRwXX7icyOuf1Pi0u6p.0FM0MCNxvxNwAyNzDzW}
pvm.bolldge{ogDnS_w8CRwWX7hbzPte1Oi1u7q.0FM0LDOxwyNwAzNzEzW}
own.college{ogEoS_x8DRxXX8iczPuf1Pi1u7q.0FM0MDOxwyMwAzNzEzW}
own.college{ogEoR_x8DRxXX8icyOuf1Pi1u7q.0FM0MCNxwxNwAzNzEzW}
own.college{ofEoS_x8DRxXX8hczPuf1Pi0u7q.0FM0MDOxwyNwAzMzEzW}
own.colldge{ogEoS_x7DQwXX8ibyPuf0Oi1u7q.0FM0MDOxvyMwAzNyDzV}
own.colldge{ngDoR_x8DRxXX8iczPuf1Pi0u7q.0FM0MCOxwxNwAzNzEzW}
own.colldfd{nfEnS_x7DRxXX8hczPue1Oh1u7q.0EM0MDOwwyMvAyNzEzW}
own.coklege{ogEoS_x8DRxXX8icyPuf1Pi1u7p.0FM0MCOxwyNwAyNzEzW}
own.coklege{ogEnR_x7DQwXX8ibzPuf0Oi1u7q.0EL0MCNxvxNwAzMyEzV}
own.cokkdge{ogDnS_w8CRxXX8iczOuf1Oh0u7p.0FM0LDNxwyMvAzMzEzW}
own.cokkdge{ngEoR_w7DRxWX7iczOtf1Oi1t7q.0FL0MDOxwyMwAyMzDzV}
own.cnlldgd{ofEnR_x8DRxXW7hbzPuf0Pi0t7q.0EM0LDNwvyNwAzNzEzW}
own.cnlldfd{nfDnR_x8DRxWX7ibzOuf0Ph1t7p.0FM0LCOxvyNvAzNzDyW}
own.bollefd{ogDoS_x7CQwWX8iczOuf0Ph1t7q.0FL0MCNwwyNvAyNzEzW}
own.bolldge{ogEoR_x8DRwXX7icyPue1Oi1u7q.0FL0MDNxwyNwAyNzDzW}
own.bolldgd{ofEnS_x8CRxWX8iczPtf1Oi0u6q.0EM0LDNxvyMwAzNzEyW}
own.bnllegd{ogEnS_x8DRxXX8iczOuf1Pi1u7q.0EM0LDOxvyNwAzNzDzW}
owm.college{ogDoS_x8DRxXX8iczOuf1Pi1u7q.0FM0MDOxwyNwAzNzEzW}
owm.bnllege{nfDoS_x8DRxXX8iczPtf0Oi1u7p.0FM0MDNxvyNwAzNyEzW}
ovn.college{ogDoR_w8DRwXW8hczPte1Pi1u7q.0FL0MDNwwxNwAyNyDzW}
ovn.college{nfEoS_x8DRxXX7iczPuf1Ph1t7q.0FM0MDOxwyNwAzNzEyW}
ovn.colldge{ofEoR_w7CRxXX7icyPtf1Pi1t7q.0FM0MDOxwyNwAyNzEzW}
ovn.coklege{ogEnS_w8CRxXX8ibyPuf1Pi1t7q.0EM0MDOxwyMvAzNzEzV}
ovn.bollege{ogEoS_x8DQxXX8hcyPuf0Oi1u6p.0FL0MDNxwxNvAyNyEyW}
ovn.bollege{ogEoS_x8CQxWX8iczPuf1Ph1u7p.0FM0MDNxvyNwAzNzDzW}
ovn.bollefe{ogEnS_w8CRwXX8ibyPuf1Oi1u7q.0FL0MDNwwyNwAyNzEyV}
ovm.cnlldge{ogEnS_x8CQwXX8icyPtf1Pi1u7q.0FM0LDOxwyMwAyNyEzW}
ovm.bolkefd{ofEoR_x8CRxWX8iczPtf1Ph1u7q.0FM0MCNwwyMwAzMzEyW}
```
I sorted the list of flags but with an argument -r so that the reverse order would appear and the top flag would be the right flag

### New Learnings
How to use the sort command to sort in various ways using different arguments