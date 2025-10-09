# Pondering PATH 

## The PATH Variable 
It turns out that the answer to "How does the shell find ls?" is fairly simple. There is a special shell variable, called PATH, that stores a bunch of directory paths in which the shell will search for programs corresponding to commands. If you blank out the variable, things go badly:
```bash
hacker@dojo:~$ ls
Desktop    Downloads  Pictures  Templates
Documents  Music      Public    Videos
hacker@dojo:~$ PATH=""
hacker@dojo:~$ ls
bash: ls: No such file or directory
hacker@dojo:~$
```
Without a PATH, bash cannot find the ls command.

In this level, you will disrupt the operation of the /challenge/run program. This program will DELETE the flag file using the rm command. However, if it can't find the rm command, the flag will not be deleted, and the challenge will give it to you! Thus, you must make it so that /challenge/run also can't find the rm command!

Keep in mind: /challenge/run will be a child process of your shell, so you must apply the concepts you learned in Shell Variables to mess with its PATH variable! If you don't succeed, and the flag gets deleted, you will need to restart the challenge to try again!

### Solve
**Flag: pwn.college{oWMg9QABGfQkz0rREV8iftqdA_a.QX2cDM1wyNwAzNzEzW}**

```bash
hacker@path~the-path-variable:~$ PATH=""
hacker@path~the-path-variable:~$ /challenge/run
Trying to remove /flag...
/challenge/run: line 4: rm: No such file or directory
The flag is still there! I might as well give it to you!
pwn.college{oWMg9QABGfQkz0rREV8iftqdA_a.QX2cDM1wyNwAzNzEzW}
```
First I emptied PATH so that no commands can be recognised including rm, so when /challenge/run runs it doesn't get to delete the flag, so when I run /challenge/run, it gives me the flag

### New Learning
How the PATH variable works.

## Setting PATH
Okay, so things break when you blank out PATH. But what about doing something useful with PATH?

Let's explore how we would, for example, add a new directory of programs to our command repertoire. Recall that PATH stores a list of directories to find commands in and, for commands in nonstandard places, we must typically execute them via their path:
```bash
hacker@dojo:~$ ls /home/hacker/scripts
goodscript	badscript	okayscript
hacker@dojo:~$ goodscript
bash: goodscript: command not found
hacker@dojo:~$ /home/hacker/scripts/goodscript
YEAH! This is the best script!
hacker@dojo:~$
```
If you maintain useful scripts that you want to be able to launch by bare name, this is annoying. However, by adding directories to or replacing directories in this list, you can expose these programs to be launched using their bare name! For example:
```bash
hacker@dojo:~$ PATH=/home/hacker/scripts
hacker@dojo:~$ goodscript
YEAH! This is the best script!
hacker@dojo:~$
```
Let's practice. This level's /challenge/run will run the win command via its bare name, but this command exists in the /challenge/more_commands/ directory, which is not initially in the PATH. The win command is the only thing that /challenge/run needs, so you can just overwrite PATH with that one directory. Good luck!

### Solve
**Flag: pwn.college{EypPJ2_0ydUsegPMOQyhePhQsGQ.QX1cjM1wyNwAzNzEzW}**

```bash
hacker@path~setting-path:~$ PATH="/challenge/more_commands/"
hacker@path~setting-path:~$ /challenge/run
Invoking 'win'....
Congratulations! You properly set the flag and 'win' has launched!
pwn.college{EypPJ2_0ydUsegPMOQyhePhQsGQ.QX1cjM1wyNwAzNzEzW}
```
First I set the /challenge/more_commands directory to PATH so that PATH variable has the command to 'win', so when /challenge/run is executed, It can invoke win without any issues and I get the flag

### New Learning
How to set the PATH to a directory.

## Finding Commands 
When you type the name of a command, something inside one of the many directories listed in your $PATH variable is what actually gets executed (of course, unless the command is a builtin!). But which file, precisely? You can find out with the aptly-named which command:
```bash
hacker@dojo:~$ which cat
/bin/cat
hacker@dojo:~$ /bin/cat /flag
YEAH
hacker@dojo:~$
```
Mirroring what the shell does when searching for commands, which looks at each directory in $PATH in order and prints the first file it finds whose name matches the argument you passed.

In this challenge, we added a win command somewhere in your $PATH, but it won't give you the flag. Instead, it's in the same directory as a flag file that we made readable by you! You must find win (with the which command), and cat the flag out of that directory!

### Solve
**Flag: pwn.college{Y4EZsDE4YjiJWeGRJeoAFKfSldo.01NzEzNxwyNwAzNzEzW}**

```bash
hacker@path~finding-commands:~$ which win
/challenge/paths/20236/win
hacker@path~finding-commands:~$ cat /challenge/paths/20236/flag
pwn.college{Y4EZsDE4YjiJWeGRJeoAFKfSldo.01NzEzNxwyNwAzNzEzW}
```
First I ran which command with the win command as its argument and then it displays the path to the win command, then as mentioned in the challenge, the flag was in the same directory so I ran flag at /challenge/paths/20236/ directory, to print it out.

### New Learning
How to use the which command to print a command's path. 

## Adding Commands 
Recall our example from the previous level:
```bash
hacker@dojo:~$ ls /home/hacker/scripts
goodscript	badscript	okayscript
hacker@dojo:~$ PATH=/home/hacker/scripts
hacker@dojo:~$ goodscript
YEAH! This is the best script!
hacker@dojo:~$
```
What we see here, of course, is the hacker making the shell more useful for themselves by bringing their own commands to the party. Over time, you might amass your own elegant tools. Let's start with win!

Previously, the win command that /challenge/run executed was stored in /challenge/more_commands. This time, win does not exist! Recall the final level of Chaining Commands, and make a shell script called win, add its location to the PATH, and enable /challenge/run to find it!

Hint: /challenge/run runs as root and will call win. Thus, win can simply cat the flag file. Again, the win command is the only thing that /challenge/run needs, so you can just overwrite PATH with that one directory. But remember, if you do that, your win command won't be able to find cat.

You have three options to avoid that:

1. Figure out where the cat program is on the filesystem. It must be in a directory that lives in the PATH variable, so you can print the variable out (refer to Shell Variables to remember how!), and go through the directories in it (recall that the different entries are separated by :), find which one has cat in it, and invoke cat by its absolute path.
2. Set a PATH that has the old directories plus a new entry for wherever you create win.
3. Use read (again, refer to Shell Variables) to read /flag. Since read is a builtin functionality of bash, it is unaffected by PATH shenanigans.
Now, go and win!

### Solve
**Flag: pwn.college{IDe_lz0XfFpsN-LBR3z_jKyVKYl.QX2cjM1wyNwAzNzEzW}**

```bash
hacker@path~adding-commands:~$ nano ~/win
  GNU nano 8.4                                              /home/hacker/win                                                            #!/bin/bash

  cat /flag
hacker@path~adding-commands:~$ chmod +x ~/win
hacker@path~adding-commands:~$ PATH="$HOME:$PATH"
hacker@path~adding-commands:~$ /challenge/run
Invoking 'win'....
pwn.college{IDe_lz0XfFpsN-LBR3z_jKyVKYl.QX2cjM1wyNwAzNzEzW}
```
First I ran nano ~/win to write a shell script for ~/win, in which I added shebangs and catting the flag, and then changed its permission to make the file executable and then, changed PATH variable to $HOME which contains the commands of the home directory(needed for the win command) and $PATH referring to the original, existing value of the PATH variable(needed for commands like ls, grep, etc but cat command for this particular circumstance), then I ran /challenge/run to invoke win and print the flag. 

### New Learning
How to add a new command aswell as keeping the old commands and how to set PATH variable for it.

### Resources
https://youtu.be/hk0RwVC6uts?si=y5bj9MS5j5Lq-taF

## Hijacking Commands 
Armed with your knowledge, you can now carry out some shenanigans. This challenge is almost the same as the first challenge in this module. Again, this challenge will delete the flag using the rm command. But unlike before, it will not print anything out for you.

How can you solve this? You know that rm is searched for in the directories listed in the PATH variable. You have experience creating the win command when the previous challenge needed it. What else can you create?


### Solve
**Flag: pwn.college{M70xm7vUKBumOQdaw0ULWJhyV4m.QX3cjM1wyNwAzNzEzW}**

```bash
hacker@path~hijacking-commands:~$ nano ~/rm
  GNU nano 8.4                                               /home/hacker/rm                                                            #!/bin/bash

  cat "$2"
hacker@path~hijacking-commands:~$ chmod +x ~/rm
hacker@path~hijacking-commands:~$ PATH="$HOME:$PATH"
hacker@path~hijacking-commands:~$  /challenge/run
Trying to remove /flag...
Found 'rm' command at /home/hacker/rm. Executing!
pwn.college{M70xm7vUKBumOQdaw0ULWJhyV4m.QX3cjM1wyNwAzNzEzW}
```
The plan was to replace the rm command from deleting the flag to catting the flag entirely, so first I ran nano ~/rm to write the shell script of the new ~/rm in the home directory, in that I ran cat "$2" because $1 was -f (Since the program was running rm -f /flag), so then I added executable permissions to the file and then set the PATH variable to $HOME(home directory and its commands) and $PATH(the old original PATH variable commands) so when it runs /challenge/run it executes, cat /flag before deletion and prints out the flag. 

### New Learning
How to hijack over certain commands.

