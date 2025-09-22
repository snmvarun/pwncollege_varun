# Pondering Paths

## The Root
Alright, so the filesystem starts at /. Under that, there are a whole mess of other directories, configuration files, programs, and, most importantly, flags. In this level, we've added a program right in /, called pwn, that will give you the flag. All you need to do for this level is to invoke this program!

You can invoke a program by providing its path on the command line. In this case, you'll be giving the exact path, starting from /, so the path would be /pwn. This style of path, one that starts with the root directory, is referred to as an "absolute path".

Start the challenge, launch a terminal, invoke the pwn program using its absolute path, and Capture that Flag! Good luck!

### Solve
**Flag: pwn.college{8jVWpU4ekO_WlYOozxPTpqFJ4dh.QX4cTO0wyNwAzNzEzW}**
```bash
hacker@paths~the-root:~$ /pwn
BOOM!!!
Here is your flag:
pwn.college{8jVWpU4ekO_WlYOozxPTpqFJ4dh.QX4cTO0wyNwAzNzEzW}
```

I invoked the program "pwn" from the filesystem starting at /, executing this line provided me with the flag

### New Learnings
Learnt how to start to invoke a program by a path on the command line, in this case from a filesystem / and program pwn

## Program and absolute paths
Let's explore a slightly more complicated path! Except for in the previous level, challenges in pwn.college are in the challenge directory and the challenge directory is, in turn, right in the root directory (/). The path to the challenge directory is, thus, /challenge. The name of the challenge program in this level is run, and it lives in the /challenge directory. Thus, the path to the run challenge program is /challenge/run.

This challenge again requires you to execute it by invoking its absolute path. You'll want to execute the run file that is in the challenge directory that is, in turn, in the / directory. If you invoke the challenge correctly, it will give you the flag. Good luck!

### Solve 
**Flag: pwn.college{YmlKqY-0Gglvor-kAE0KRT9ZU7Y.QX1QTN0wyNwAzNzEzW}**

```bash
hacker@paths~program-and-absolute-paths:~$ /challenge/run
Correct!!!
/challenge/run is an absolute path! Here is your flag:
pwn.college{YmlKqY-0Gglvor-kAE0KRT9ZU7Y.QX1QTN0wyNwAzNzEzW}
```
I executed statements to follow and invoke its absolute path, with /challenge being root directory and /run a program inside the /challenge path

### New learnings
To execute a file in its directory.

## Position Thy Self 
The Linux filesystem has tons of directories with tons of files. You can navigate around directories by using the cd (change directory) command and passing a path to it as an argument, as so:

hacker@dojo:~$ cd /some/new/directory
hacker@dojo:/some/new/directory$
This affects the "current working directory" of your process (in this case, the bash shell). Each process has a directory in which it's currently hanging out. The reasons for this will become clear later in the module.

As an aside, now you can see what the ~ was in the prompt! It shows the current path that your shell is located at.

This challenge will require you to execute the /challenge/run program from a specific path (which it will tell you). You'll need to cd to that directory before rerunning the challenge program. Good luck!

### Solve 
**Flag: pwn.college{YTu2XizQD17j-IgRqnXLmvB43ga.QX2QTN0wyNwAzNzEzW}**

```bash
hacker@paths~position-thy-self:~$ cd /home
hacker@paths~position-thy-self:/home$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Here is your flag:
pwn.college{YTu2XizQD17j-IgRqnXLmvB43ga.QX2QTN0wyNwAzNzEzW}
```

Changed the directory to /home and then executed the program from a specific path.

### New Learnings
How to change directories and execute from a specific path

## Position Elsewhere
The Linux filesystem has tons of directories with tons of files. You can navigate around directories by using the cd (change directory) command and passing a path to it as an argument, as so:

hacker@dojo:~$ cd /some/new/directory
hacker@dojo:/some/new/directory$
This affects the "current working directory" of your process (in this case, the bash shell). Each process has a directory in which it's currently hanging out. The reasons for this will become clear later in the module.

As an aside, now you can see what the ~ was in the prompt! It shows the current path that your shell is located at.

This challenge will require you to execute the /challenge/run program from a specific path (which it will tell you). You'll need to cd to that directory before rerunning the challenge program. Good luck!

### Solve
**Flag: pwn.college{MFv1ClKaily7Lyi6Soop8kjEmsD.QX3QTN0wyNwAzNzEzW}**

```bash
hacker@paths~position-elsewhere:~$ /challenge/run
Incorrect...
You are not currently in the /var/lib/apt/lists directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~position-elsewhere:~$ cd /var/lib/apt/lists
hacker@paths~position-elsewhere:/var/lib/apt/lists$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Here is your flag:
pwn.college{MFv1ClKaily7Lyi6Soop8kjEmsD.QX3QTN0wyNwAzNzEzW}
```
Changed the directory to /var/lib/apt/lists and then executed the program from a specific path.

### New learnings
How to change directories and execute from a specific path.

## Position yet elsewhere
The Linux filesystem has tons of directories with tons of files. You can navigate around directories by using the cd (change directory) command and passing a path to it as an argument, as so:

hacker@dojo:~$ cd /some/new/directory
hacker@dojo:/some/new/directory$
This affects the "current working directory" of your process (in this case, the bash shell). Each process has a directory in which it's currently hanging out. The reasons for this will become clear later in the module.

As an aside, now you can see what the ~ was in the prompt! It shows the current path that your shell is located at.

This challenge will require you to execute the /challenge/run program from a specific path (which it will tell you). You'll need to cd to that directory before rerunning the challenge program. Good luck!

### Solve
**Flag: pwn.college{UJrsQOtC50R7wKccMoBjN0s2Mmt.QX4QTN0wyNwAzNzEzW}**

```bash
hacker@paths~position-yet-elsewhere:~$ /challenge/run
Incorrect...
You are not currently in the /sys/kernel directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~position-yet-elsewhere:~$ cd /sys/kernel
hacker@paths~position-yet-elsewhere:/sys/kernel$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Here is your flag:
pwn.college{UJrsQOtC50R7wKccMoBjN0s2Mmt.QX4QTN0wyNwAzNzEzW}
```

Changed the directory to /sys/kernel and then executed the program from a specific path.

### New Learnings
How to change directories and execute from a specific path.

## implicit relative path, from /
Now you're familiar with the concept of referring to absolute paths and changing directories. If you put in absolute paths everywhere, then it really doesn't matter what directory you are in, as you likely found out in the previous three challenges.

However, the current working directory does matter for relative paths.

A relative path is any path that does not start at root (i.e., it does not start with /).
A relative path is interpreted relative to your current working directory (cwd).
Your cwd is the directory that your prompt is currently located at.
This means how you specify a particular file, depends on where the terminal prompt is located.

Imagine we want to access some file located at /tmp/a/b/my_file.

If my cwd is /, then a relative path to the file is tmp/a/b/my_file.
If my cwd is /tmp, then a relative path to the file is a/b/my_file.
If my cwd is /tmp/a/b/c, then a relative path to the file is ../my_file. The .. refers to the parent directory.
Let's try it here! You'll need to run /challenge/run using a relative path while having a current working directory of /. For this level, I'll give you a hint. Your relative path starts with the letter c 😊

### Solve
***Flag: pwn.college{8A26NfycYIiKslElxIQ2qIW5Qiq.QX5QTN0wyNwAzNzEzW}**

```bash
hacker@paths~implicit-relative-paths-from-:~$ cd /
hacker@paths~implicit-relative-paths-from-:/$ challenge/run
Correct!!!
challenge/run is a relative path, invoked from the right directory!
Here is your flag:
pwn.college{8A26NfycYIiKslElxIQ2qIW5Qiq.QX5QTN0wyNwAzNzEzW}
```
Ran "challenge/run" using the relative path having cwd of /

### New Learnings 
How to execute a file stored in a directory using a relative path to that directory.

## explicit relative path, from /
### Solve
**Flag: pwn.college{Qn3kvVpVTEtmSjUz09EOplkz9bR.QXwUTN0wyNwAzNzEzW}**

```bash
hacker@paths~explicit-relative-paths-from-:~$ cd /
hacker@paths~explicit-relative-paths-from-:/$ ./challenge/run
Correct!!!
./challenge/run is a relative path, invoked from the right directory!
Here is your flag:
pwn.college{Qn3kvVpVTEtmSjUz09EOplkz9bR.QXwUTN0wyNwAzNzEzW}
``` 

### New Learnings 
How to execute a file stored in a directory using a explicit relative path to that directory.

## implicit relative path
In this level, we'll practice referring to paths using . a bit more. This challenge will need you to run it from the /challenge directory. Here, things get slightly tricky.

Linux explicitly avoids automatically looking in the current directory when you provide a "naked" path. Consider the following:

hacker@dojo:~$ cd /challenge
hacker@dojo:/challenge$ run
This will not invoke /challenge/run. This is actually a safety measure: if Linux searched the current directory for programs every time you entered a naked path, you could accidentally execute programs in your current directory that happened to have the same names as core system utilities! As a result, the above commands will yield the following error:

bash: run: command not found
We'll explore the mechanisms behind this concept later, but in this challenge, we'll learn how to explicitly use relative paths to launch run in this scenario. The way to do this is to tell Linux that you explicitly want to execute a program in the current directory, using . like in the previous levels. Give it a try now!

### Solve
**Flag: pwn.college{8FxPZAm1njO5gLFrm-jvjE_1DUt.QXxUTN0wyNwAzNzEzW}**

```bash
hacker@paths~implicit-relative-path:~$ cd /challenge
hacker@paths~implicit-relative-path:/challenge$ /run
bash: /run: Is a directory
hacker@paths~implicit-relative-path:/challenge$ run
bash: run: command not found
hacker@paths~implicit-relative-path:/challenge$ ./run
Correct!!!
./run is a relative path, invoked from the right directory!
Here is your flag:
pwn.college{8FxPZAm1njO5gLFrm-jvjE_1DUt.QXxUTN0wyNwAzNzEzW}
```

### New Learnings
How to execute a file stored in a directory using a explicit relative path to that directory.

## Home sweet home
Every user has a home directory, typically under /home in the filesystem. In the dojo, you are the hacker user, and your home directory is /home/hacker. The home directory is typically where users store most of their personal files. As you make your way through pwn.college, this is where you'll store most of your solutions.

Typically, your shell session will start with your home directory as your current working directory. Consider the initial prompt:

hacker@dojo:~$
The ~ in this prompt is the current working directory, with ~ being shorthand for /home/hacker. Bash provides and uses this shorthand because, again, most of your time will be spent in your home directory. Thus, whenever bash sees ~ provided as the start of an argument in a way consistent with a path, it will expand it to your home directory. Consider:

hacker@dojo:~$ echo LOOK: ~
LOOK: /home/hacker
hacker@dojo:~$ cd /
hacker@dojo:/$ cd ~
hacker@dojo:~$ cd ~/asdf
hacker@dojo:~/asdf$ cd ~/asdf
hacker@dojo:~/asdf$ cd ~
hacker@dojo:~$ cd /home/hacker/asdf
hacker@dojo:~/asdf$
Note that the expansion of ~ is an absolute path, and only the leading ~ is expanded. This means, for example, that ~/~ will be expanded to /home/hacker/~ rather than /home/hacker/home/hacker.

Fun fact: cd will use your home directory as the default destination:

hacker@dojo:~$ cd /tmp
hacker@dojo:/tmp$ cd
hacker@dojo:~$
Now it's your turn to play! In this challenge, /challenge/run will write a copy of the flag to any file you specify as an argument on the commandline, with these constraints:

Your argument must be an absolute path.
The path must be inside your home directory.
Before expansion, your argument must be three characters or less.
Again, you must specify your path as an argument to /challenge/run as so:

hacker@dojo:~$ /challenge/run YOUR_PATH_HERE

### Solve
**Flag: pwn.college{EW-8QIm_Gk-6y20nUoYHvoGIJIU.QXzMDO0wyNwAzNzEzW}**

```bash
hacker@paths~home-sweet-home:~$ /challenge/run ~/r
Writing the file to /home/hacker/r!
... and reading it back to you:
pwn.college{EW-8QIm_Gk-6y20nUoYHvoGIJIU.QXzMDO0wyNwAzNzEzW}
```

### New Learnings
Learned how to invoke a command with a file path as an argument.

