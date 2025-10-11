# Daring Destruction

## The Fork Bomb
As you learned in the Processes and Jobs module, whenever you start a program the Linux operating system creates a new process. If you create processes faster than the kernel can handle, the process table fills up and everything grinds to a halt. This new process (e.g., of an ls invocation) is ``forked'' off of a parent process (e.g., a shell instance). Thus, the induced explosion of processes is called a "Fork Bomb".

You have the tools to do this:

- write a small script (like in the Chaining Commands module)
- make it executable (like in the Perceiving Permissions module)
- make it launch a copy of itself in the background (like in the Processes and Jobs module)
- and then launch another copy of itself in the background!
Each copy will launch two more, and each of those will launch two more, and you will flood the system with so many processes that new ones will not be able to start!

This challenge contains a /challenge/check that'll try to determine if your fork bomb is working (e.g., if it can't launch new processes) and give you the flag if so. Make sure to launch it (in a different terminal) before launching your attack; otherwise you won't be able to launch it!

NOTE: Needless to say, this will render your environment unusable. Just restart the challenge (or start a different one) to get things back to a usable state!


### Solve
**Flag: pwn.college{gsMsw22EjJDglPFF-h9dlwhWj9O.0VMyEzNxwyNwAzNzEzW}**

```bash
#Terminal 1
hacker@destruction~the-fork-bomb:~$ nano fork_bomb.sh
  GNU nano 8.4                                                fork_bomb.sh                                                             :(){ :|:& };:
hacker@destruction~the-fork-bomb:~$ chmod +x fork_bomb.sh
hacker@destruction~the-fork-bomb:~$ ./fork_bomb.sh
./fork_bomb.sh: fork: retry: Resource temporarily unavailable
./fork_bomb.sh: fork: retry: Resource temporarily unavailable
./fork_bomb.sh: fork: retry: Resource temporarily unavailable
./fork_bomb.sh: fork: retry: Resource temporarily unavailable
./fork_bomb.sh: fork: retry: Resource temporarily unavailable
./fork_bomb.sh: fork: retry: Resource temporarily unavailable
./fork_bomb.sh: fork: retry: Resource temporarily unavailable
./fork_bomb.sh: fork: retry: Resource temporarily unavailable
./fork_bomb.sh: fork: retry: Resource temporarily unavailable
./fork_bomb.sh: fork: retry: Resource temporarily unavailable
./fork_bomb.sh: fork: retry: Resource temporarily unavailable
./fork_bomb.sh: fork: retry: Resource temporarily unavailable
```

```bash
#Terminal 2
hacker@destruction~the-fork-bomb:~$ /challenge/check
It looks like the system can still spawn processes. We'll check again in 5 seconds...
It looks like the system can still spawn processes. We'll check again in 5 seconds...
You successfully saturated the process table.  Here is your hard-earned flag:
pwn.college{gsMsw22EjJDglPFF-h9dlwhWj9O.0VMyEzNxwyNwAzNzEzW}
```
I ran the fork bomb with the code " :(){ :|:& };: " for denial of service and set the permissions of fork bomb to be executable and executed it to flood the terminal with fork bombs, which in the second terminal, successfully completed the check and gave the flag.

### New Learnings
How to fork-bomb and how does it work.

### Resources
https://youtu.be/RhtjGp7oMvE?si=O0qDkmVq1a0V-4MU

## Disc-Space Doomsday 
The available space in /home/hacker in this container is a measly 1 gigabyte. In this level you will clog up /home/hacker with so much junk that even a tiny 1 megabyte file can't be created. When this happens, your workspace becomes unusable. We'll practice inducing this in this challenge, and then expand on it a bit later.

How to fill the disk? There are so many ways. Here, we'll teach you the yes command!
```bash
hacker@dojo:~$ yes | head
y
y
y
y
y
y
y
y
y
y
hacker@dojo:~$
```
The yes outputs y over and over forever. The typical usage is to automate confirmation prompts ("Are you sure you want to delete this file?") using piping, but we'll use it here to make a massive file full of "y" lines. Just redirect yes to a file in your home directory, and you'll fill your disk in a minute or two!

This challenge forces you to fill the disk and then clean up. The process:

1. Fill your disk.
2. Run /challenge/check. It will attempt to create a 1 megabyte temporary file. If that fails, you pass the first stage and the checker will ask you to free the space.
3. Delete the file you made (with rm) to clear up the space.
4. Run /challenge/check a second time. If it can now create the temporary file (i.e., you successfully cleaned up your home directory), you’ll receive the flag.
Why two stages? Your home directory persists across challenge instances. If we let you keep it full, your pwn.college will stop working. This is by far the most common cause of weird issues on pwn.college!

HELP IT BROKE! If you fill the disk and don't clean it up afterwards, you'll need to ssh in to fix things (by removing that file). This is a bit tricky, but we describe how to do it under "Connecting over SSH" in the Getting Started module.

### Solve
**Flag: pwn.college{I3wgqDFIZw0Up6B0p2YwSXP7-X-.0lMyEzNxwyNwAzNzEzW}**

```bash
hacker@destruction~disk-space-doomsday:~$ yes > junk_file
yes: standard output: Disk quota exceeded
hacker@destruction~disk-space-doomsday:~$ /challenge/check
Well done, you clogged the disk. Now free that space (remove the file you created) and run /challenge/check again to prove you cleaned up!
hacker@destruction~disk-space-doomsday:~$ rm junk_file
hacker@destruction~disk-space-doomsday:~$ /challenge/check
Disk space restored. Here is your flag:
pwn.college{I3wgqDFIZw0Up6B0p2YwSXP7-X-.0lMyEzNxwyNwAzNzEzW}
```
First I used the yes command and generated an endless stream of text into the junk_file file, then I ran /challenge/check to show that I've clogged the disc and then removed the junk file and ran the check again to obtain the flag.

### New Learnings
How to clog up discs with junk. 

## rm -rf /
Want to wipe the slate clean and start over? You can!
```bash
hacker@dojo:~$ ls /
bin etc blah blah blah
hacker@dojo:~$ rm -rf /
hacker@dojo:~$ ls /
bash: ls: command not found
hacker@dojo:~$
```
What happened here? As you recall, rm removes files. The -r (recursive) flag removes directories and all files containing them. The -f (force) flag ignores any errors the rm command runs into or compulsions that it may have. Combined and aimed at /, the results are catastrophic: a full wipe of your system. On a modern system, things aren't that simple, but you'll figure that out when you see it.

In this challenge, you will do something that you might never do again: wipe the whole system. We've actually modified things a bit to keep your home directory safe (normally, it would get wiped as well!), but otherwise, all that stands before you and the flag is your willingness to wipe the drive. But before you wipe it all, make sure to start /challenge/check so that it can watch the fireworks (and give you the flag)!

NOTE: The rm will take a while to run. There's a lot to delete!

NOTE: There are various technical reasons why you're unlikely to be able to delete everything, including the technique we use to protect your home directory in this level. Don't worry, you'll be doing enough damage!

### Solve
**Flag: pwn.college{Mp5rEFStmbAnaIiDSenr30QbmQN.0lMzEzNxwyNwAzNzEzW}**

```bash
#Terminal 1
hacker@destruction~rm-rf-:~$ rm -rf /
/bin/rm: it is dangerous to operate recursively on '/'
/bin/rm: use --no-preserve-root to override this failsafe
hacker@destruction~rm-rf-:~$ rm -rf --no-preserve-root /
/bin/rm: cannot remove '/etc/hosts': Device or resource busy
/bin/rm: cannot remove '/etc/resolv.conf': Device or resource busy
/bin/rm: cannot remove '/etc/hostname': Device or resource busy
/bin/rm: cannot remove '/usr/sbin/docker-init': Device or resource busy
/bin/rm: skipping '/sys', since it's on a different device
/bin/rm: skipping '/home/hacker', since it's on a different device
/bin/rm: skipping '/run/dojo/sys', since it's on a different device
/bin/rm: skipping '/proc', since it's on a different device
/bin/rm: skipping '/dev', since it's on a different device
/bin/rm: skipping '/nix', since it's on a different device
```

```bash
#Terminal 2
hacker@destruction~rm-rf-:~$ /challenge/check
Looks like you haven't wiped the system! We'll check again in 5 seconds...
Looks like you haven't wiped the system! We'll check again in 5 seconds...
Looks like you haven't wiped the system! We'll check again in 5 seconds...
Looks like you haven't wiped the system! We'll check again in 5 seconds...
Looks like you haven't wiped the system! We'll check again in 5 seconds...
YES! You wiped it, you wild hacker! The flag is yours:
pwn.college{Mp5rEFStmbAnaIiDSenr30QbmQN.0lMzEzNxwyNwAzNzEzW}
```
I ran rm -rf / but it gave me another argument to run to override the failsafe, then I ran that to delete all the system files at the home directory and then I ran /challenge/check in another terminal and obtained the flag when the files were wiped.

### New Learnings
How to run rm -rf / and each command and argument's significance, and its function

## Life after rm -rf /
Let's dig into the effects of blowing away your whole filesystem. You're now an experienced rmer, but previously, /challenge/check printed the flag out for you when you cleared away the clutter of the filesystem. What if it hadn't? Without cat, how would you read that /flag?

Recall, from the Digesting Documentation module, that some shell commands are builtins. While ls, cat, and such aren't, read (which, if you recall from the Shell Variables module, can read files!) is. That means that, even if you blow away your whole filesystem, as long as you have an already-running instance of bash, you can read files!

This challenge will force you to try it. It's almost the same as the previous one, but you must read the flag yourself after you destroy the system. After you rm everything, your previously-launched /challenge/check will restore the /flag file and make it readable. Then you can read it!

### Solve
**Flag: pwn.college{gqmbyJ7nKVC33M_FQjJRE-apOU6.01MzEzNxwyNwAzNzEzW}**

```bash
#Terminal 1
hacker@destruction~rm-rf-:~$ rm -rf /
/bin/rm: it is dangerous to operate recursively on '/'
/bin/rm: use --no-preserve-root to override this failsafe
hacker@destruction~rm-rf-:~$ rm -rf --no-preserve-root /
/bin/rm: cannot remove '/etc/hosts': Device or resource busy
/bin/rm: cannot remove '/etc/resolv.conf': Device or resource busy
/bin/rm: cannot remove '/etc/hostname': Device or resource busy
/bin/rm: cannot remove '/usr/sbin/docker-init': Device or resource busy
/bin/rm: skipping '/sys', since it's on a different device
/bin/rm: skipping '/home/hacker', since it's on a different device
/bin/rm: skipping '/run/dojo/sys', since it's on a different device
/bin/rm: skipping '/proc', since it's on a different device
/bin/rm: skipping '/dev', since it's on a different device
/bin/rm: skipping '/nix', since it's on a different device
```

```bash
#Terminal 2
hacker@destruction~rm-rf-:~$ /challenge/check
Looks like you haven't wiped the system! We'll check again in 5 seconds...
Looks like you haven't wiped the system! We'll check again in 5 seconds...
Looks like you haven't wiped the system! We'll check again in 5 seconds...
Looks like you haven't wiped the system! We'll check again in 5 seconds...
Looks like you haven't wiped the system! We'll check again in 5 seconds...
YES! You did it again! Go read the flag!
hacker@destruction~life-after-rm-rf-:~$ read flag1 < /flag
hacker@destruction~life-after-rm-rf-:~$ echo $flag1
pwn.college{gqmbyJ7nKVC33M_FQjJRE-apOU6.01MzEzNxwyNwAzNzEzW}
```
I ran the same process as the last challenge, then to print the flag since read command was still available, I inputted the output of /flag into the variable flag1 and then echoed it to read out the flag contents.

### New Learnings
I learnt that shell builtin commands like read stay even after deleting all the files in the system home directory unlike seperate commands that are in files.

## Finding meaning after rm -rf /
So you can live without cat! How about without ls? This time, /challenge/check will restore the flag to a randomly-named file. You'll need to find it without reaching for your ls command.

There are a lot of ways to solve this challenge. echo is a builtin, and you can File Glob an argument to it to expand to all files! For example, echo * will print out the names of all of the files in the current directory. Similarly, you can use tab-completion (hit tab a few times) of an argument to have the shell list possible files for you.

Whatever route you use, find the randomly-named file that /challenge/check makes in / after you rm, read it, and get the flag!

### Solve
**Flag: pwn.college{UdDNIugp7UyLVfykebC_DFOzTJj.0FNzEzNxwyNwAzNzEzW}**

```bash
#Terminal 1
hacker@destruction~rm-rf-:~$ rm -rf /
/bin/rm: it is dangerous to operate recursively on '/'
/bin/rm: use --no-preserve-root to override this failsafe
hacker@destruction~rm-rf-:~$ rm -rf --no-preserve-root /
/bin/rm: cannot remove '/etc/hosts': Device or resource busy
/bin/rm: cannot remove '/etc/resolv.conf': Device or resource busy
/bin/rm: cannot remove '/etc/hostname': Device or resource busy
/bin/rm: cannot remove '/usr/sbin/docker-init': Device or resource busy
/bin/rm: skipping '/sys', since it's on a different device
/bin/rm: skipping '/home/hacker', since it's on a different device
/bin/rm: skipping '/run/dojo/sys', since it's on a different device
/bin/rm: skipping '/proc', since it's on a different device
/bin/rm: skipping '/dev', since it's on a different device
/bin/rm: skipping '/nix', since it's on a different device
```

```bash
#Terminal 2
hacker@destruction~rm-rf-:~$ /challenge/check
Looks like you haven't wiped the system! We'll check again in 5 seconds...
Looks like you haven't wiped the system! We'll check again in 5 seconds...
Looks like you haven't wiped the system! We'll check again in 5 seconds...
Looks like you haven't wiped the system! We'll check again in 5 seconds...
Looks like you haven't wiped the system! We'll check again in 5 seconds...
YES! You did it again! Go read the flag!
hacker@destruction~finding-meaning-after-rm-rf-:~$ echo /*
/2c48ec3a /dev /etc /home /nix /proc /run /sys /usr
hacker@destruction~finding-meaning-after-rm-rf-:~$ read flag1 < /2c48ec3a
hacker@destruction~finding-meaning-after-rm-rf-:~$ echo $flag1
pwn.college{UdDNIugp7UyLVfykebC_DFOzTJj.0FNzEzNxwyNwAzNzEzW}
```
I ran the same process as the last challenge, then after all the system files were deleted, I echoed the contents of / directory(similar to C) to find a code looking / file and when I catted it using the restrictions of the builtin commands, I got the flag

### New Learnings
How to list without ls command using shell builtins.  

