# File Globbing 
## Matching with *
The first glob we'll learn is *. When it encounters a * character in any argument, the shell will treat it as a "wildcard" and try to replace that argument with any files that match the pattern. It's easier to show you than explain:
```bash
hacker@dojo:~$ touch file_a
hacker@dojo:~$ touch file_b
hacker@dojo:~$ touch file_c
hacker@dojo:~$ ls
file_a	file_b	file_c
hacker@dojo:~$ echo Look: file_*
Look: file_a file_b file_c
```

Of course, though in this case, the glob resulted in multiple arguments, it can just as simply match only one. For example:

```bash
hacker@dojo:~$ touch file_a
hacker@dojo:~$ ls
file_a
hacker@dojo:~$ echo Look: file_*
Look: file_a
```

When zero files are matched, by default, the shell leaves the glob unchanged:

```bash
hacker@dojo:~$ touch file_a
hacker@dojo:~$ ls
file_a
hacker@dojo:~$ echo Look: nope_*
Look: nope_*
```

The * matches any part of the filename except for / or a leading . character. For example:

```bash
hacker@dojo:~$ echo ONE: /ho*/*ck*
ONE: /home/hacker
hacker@dojo:~$ echo TWO: /*/hacker
TWO: /home/hacker
hacker@dojo:~$ echo THREE: ../*
THREE: ../hacker
```
Now, practice this yourself! Starting from your home directory, change your directory to /challenge, but use globbing to keep the argument you pass to cd to at most four characters! Once you're there, run /challenge/run for the flag!

### Solve
**Flag: pwn.college{EcOJXd8LRlharY_QNHBX0N3MzJ9.QXxIDO0wyNwAzNzEzW}**

```bash 
hacker@globbing~matching-with-:~$ cd /ch*
hacker@globbing~matching-with-:/challenge$ /c*/r*
You ran me with the working directory of /challenge! Here is your flag:
pwn.college{EcOJXd8LRlharY_QNHBX0N3MzJ9.QXxIDO0wyNwAzNzEzW}
```

First I changed my directory to /challenge by /ch* which consist of 4 characters as mentioned in the condition, since the * autofills in the most appropriate pattern, then similarly i ran the working directory, where * in /c* fills in the pattern for /challenge and /r* fills in for /run

### New Learning 
What are globbings and how the the glob * works

## Matching with ? 
Next, let's learn about ?. When it encounters a ? character in any argument, the shell will treat it as a single-character wildcard. This works like *, but only matches one character. For example:

```bash
hacker@dojo:~$ touch file_a
hacker@dojo:~$ touch file_b
hacker@dojo:~$ touch file_cc
hacker@dojo:~$ ls
file_a	file_b	file_cc
hacker@dojo:~$ echo Look: file_?
Look: file_a file_b
hacker@dojo:~$ echo Look: file_??
Look: file_cc
```

Now, practice this yourself! Starting from your home directory, change your directory to /challenge, but use the ? character instead of c and l in the argument to cd! Once you're there, run /challenge/run for the flag!

### Solve
**Flag: pwn.college{4G14XMK9l94_kPlZ3SYUedj5lyi.QXyIDO0wyNwAzNzEzW}**

```bash
hacker@globbing~matching-with-:~$ cd /?ha??enge
hacker@globbing~matching-with-:/challenge$ /challenge/run
You ran me with the working directory of /challenge! Here is your flag:
pwn.college{4G14XMK9l94_kPlZ3SYUedj5lyi.QXyIDO0wyNwAzNzEzW}
```

I changed the directory to /challenge by following the conditions and used ? in place of 'c' and 'l' so they could autofill the character with the most appropriate pattern, after changing the directory I ran /challenge/run to obtain the flag

### New Learnings
How the ? glob work

## Matching with []
Next, we will cover []. The square brackets are, essentially, a limited form of ?, in that instead of matching any character, [] is a wildcard for some subset of potential characters, specified within the brackets. For example, [pwn] will match the character p, w, or n. For example:

```bash
hacker@dojo:~$ touch file_a
hacker@dojo:~$ touch file_b
hacker@dojo:~$ touch file_c
hacker@dojo:~$ ls
file_a	file_b	file_c
hacker@dojo:~$ echo Look: file_[ab]
Look: file_a file_b
```

Try it here! We've placed a bunch of files in /challenge/files. Change your working directory to /challenge/files and run /challenge/run with a single argument that bracket-globs into file_b, file_a, file_s, and file_h!

### Solve
**Flag: pwn.college{gifkTeMksZ0d2StZFdez-VLfWRY.QXzIDO0wyNwAzNzEzW}**

```bash
hacker@globbing~matching-with-:~$ cd /challenge/files
hacker@globbing~matching-with-:/challenge/files$ ls
file_a  file_c  file_e  file_g  file_i  file_k  file_m  file_o  file_q  file_s  file_u  file_w  file_y
file_b  file_d  file_f  file_h  file_j  file_l  file_n  file_p  file_r  file_t  file_v  file_x  file_z
hacker@globbing~matching-with-:/challenge/files$ /challenge/run file_[abhs]
You got it! Here is your flag!
pwn.college{gifkTeMksZ0d2StZFdez-VLfWRY.QXzIDO0wyNwAzNzEzW}
```
First I changed my directory to /challenge/files, then I checked the listing to find files with letters a-z, therefore I ran the file in
/challenge/run with the glob [absh] to get the specfic files ending with 'a','b','s','h'.

### New Learnings
How the [] glob works.

## Matching paths with []
Globbing happens on a path basis, so you can expand entire paths with your globbed arguments. For example:

```bash
hacker@dojo:~$ touch file_a
hacker@dojo:~$ touch file_b
hacker@dojo:~$ touch file_c
hacker@dojo:~$ ls
file_a	file_b	file_c
hacker@dojo:~$ echo Look: /home/hacker/file_[ab]
Look: /home/hacker/file_a /home/hacker/file_b
```

Now it's your turn. Once more, we've placed a bunch of files in /challenge/files. Starting from your home directory, run /challenge/run with a single argument that bracket-globs into the absolute paths to the file_b, file_a, file_s, and file_h files!

### Solve
**Flag: pwn.college{ozHhqsbgI9xHI-xYt8iLx1WRhBa.QX0IDO0wyNwAzNzEzW}**

```bash
hacker@globbing~matching-paths-with-:~$ /challenge/run /challenge/files/file_[abhs]
You got it! Here is your flag!
pwn.college{ozHhqsbgI9xHI-xYt8iLx1WRhBa.QX0IDO0wyNwAzNzEzW}
```
I invoked the /challenge/run with the directory of all the specified files(file_a,file_b,file_h,file_s) inside the /challenge/files directory with their path - /challenge/files/files/file_[abhs]

### New Learnings 
How to match paths using the [] glob

## Multiple globs
So far, you've specified one glob at a time, but you can do more! Bash supports the expansion of multiple globs in a single word. For example:

```bash
hacker@dojo:~$ cat /*fl*
pwn.college{YEAH}
hacker@dojo:~$
What happens above is that the shell looks for all files in / that start with anything (including nothing), then have an f and an l, and end in anything (including ag, which makes flag).
```

Now you try it. We put a few happy, but diversely-named files in /challenge/files. Go cd there and run /challenge/run, providing a single argument: a short (3 characters or less) globbed word with two * globs in it that covers every word that contains the letter p.

### Solve
**Flag: pwn.college{0qQ7aT2b7hzcDEcZXIGiK6YZs9T.0lM3kjNxwyNwAzNzEzW}**

```bash
hacker@globbing~multiple-globs:~$ cd /challenge/files
hacker@globbing~multiple-globs:/challenge/files$ /challenge/run *p*
You got it! Here is your flag!
pwn.college{0qQ7aT2b7hzcDEcZXIGiK6YZs9T.0lM3kjNxwyNwAzNzEzW}
```

First I changed the directory to /challenge/files, then ran /challenge/run with the arguement "*p*" with two globs, so that the arguement can find all the files that contain the letter p, and then obtain the flag.

### New Learnings
How to use multiple globs and the use of two *.

## Mixing Globs
Now, let's put the previous levels together! We put a few happy, but diversely-named files in /challenge/files. Go cd there and, using the globbing you've learned, write a single, short (6 characters or less) glob that (when passed as an argument to /challenge/run) will match the files "challenging", "educational", and "pwning"!  

### Solve
**Flag:pwn.college{Y0zC4xq_d6Pq_Q5mvTMZCRouXcQ.QX1IDO0wyNwAzNzEzW}**

```bash
hacker@globbing~mixing-globs:~$ cd /challenge/files
hacker@globbing~mixing-globs:/challenge/files$ ls
amazing      delightful   great       jovial    magical     pwning   splendid   victorious  youthful
beautiful    educational  happy       kind      nice        queenly  thrilling  wonderful   zesty
challenging  fantastic    incredible  laughing  optimistic  radiant  uplifting  xenial
hacker@globbing~mixing-globs:/challenge/files$ /challenge/run [cep]*
You got it! Here is your flag!
pwn.college{Y0zC4xq_d6Pq_Q5mvTMZCRouXcQ.QX1IDO0wyNwAzNzEzW}
```

First I changed my directory to /challenge/files, then I checked the files of the directory by using ls noticing a property that the first letter of the words we specifically want are unique. Therefore, I ran the arguement [cep] that signify the first letters of the words we want, and then added a * at the end to make sure its the first letter is the one being recognised for a pattern, and ran it in /challenge/flag to get the flag.

### New Learnings
Better Understanding in how to use multiple globs

## Exclusionary globbing
Sometimes, you want to filter out files in a glob! Luckily, [] helps you do just this. If the first character in the brackets is a ! or (in newer versions of bash) a ^, the glob inverts, and that bracket instance matches characters that aren't listed. For example:

```bash
hacker@dojo:~$ touch file_a
hacker@dojo:~$ touch file_b
hacker@dojo:~$ touch file_c
hacker@dojo:~$ ls
file_a	file_b	file_c
hacker@dojo:~$ echo Look: file_[!ab]
Look: file_c
hacker@dojo:~$ echo Look: file_[^ab]
Look: file_c
hacker@dojo:~$ echo Look: file_[ab]
Look: file_a file_b
Armed with this knowledge, go forth to /challenge/files and run /challenge/run with all files that don't start with p, w, or n!
```

NOTE: The ! character has a different special meaning in bash when it's not the first character of a [] glob, so keep that in mind if things stop making sense! ^ does not have this problem, but is also not compatible with older shells.

### Solve
**Flag:pwn.college{EctQ1lceFduR4s-cupZtau_FI60.QX2IDO0wyNwAzNzEzW}**

```bash
hacker@globbing~exclusionary-globbing:~$ cd /challenge/files
hacker@globbing~exclusionary-globbing:/challenge/files$ /challenge/run [!pwn]*
You got it! Here is your flag!
pwn.college{EctQ1lceFduR4s-cupZtau_FI60.QX2IDO0wyNwAzNzEzW}
```

First I changed my directory to /challenge/files,then I ran the argument [!pwn] ending with * to list out all the files starting without pwn 

### New Learnings
Better understanding in how to use multiple globs and the learnt the purpose of ! or ^ in a glob

## Tab Completion
As tempting as it might be, using * to shorten what must be typed on the commandline can lead to mistakes. Your glob might expand to unintended files, and you might not spot it until the rm command is already running! No one is safe from this style of error.

A safer alternative when you are trying to specify a specific target is tab completion. If you hit tab in the shell, it'll try to figure out what you're going to type and automatically complete it. Auto-completion is super useful, and this challenge will explore its use in specifying files.

This challenge has copied the flag into /challenge/pwncollege, and you can freely cat that file. But you can't type the filename: we used some serious trickery to make sure that you must tab-complete it. Try it out!

```bash
hacker@dojo:~$ ls /challenge
DESCRIPTION.md  pwncollege
hacker@dojo:~$ cat /challenge/pwncollege
cat: /challenge/pwncollege: No such file or directory
hacker@dojo:~$ cat /challenge/pwn<TAB>
pwn.college{HECK YEAH}
hacker@dojo:~$
```

When you hit that tab key, the name will expand and you'll be able to read the file. Good luck!

### Solve
**Flag: pwn.college{ce5K9LR7ww2I0VkMeO_ADeeTzO9.0FN0EzNxwyNwAzNzEzW}**

```bash
hacker@globbing~tab-completion:~$ ls /challenge
DESCRIPTION.md  pwncollege​
hacker@globbing~tab-completion:~$ cat /challenge/pwncollege​
pwn.college{ce5K9LR7ww2I0VkMeO_ADeeTzO9.0FN0EzNxwyNwAzNzEzW}
```

First I checked if there are any other files similar in /challenge in the listings, then I wrote out /challenge/pw and then clicked the tab button on the keyboard to auto-complete pwncollege, and printed the flag

### New Learnings 
Learnt how the tab button can auto-complete commands and save time

## Multiple options for tab completion 
Consider the following situation:

```bash
hacker@dojo:~$ ls
flag  flamingo  flowers
hacker@dojo:~$ cat f<TAB>
There are multiple options! What happens?
```

What happens varies based on the specific shell and its options. By default bash will auto-expand until the first point when there are multiple options (in this case, fl). When you hit tab a second time, it'll print out those options. Other shells and configurations, instead, will cycle through the options.

This challenge has a /challenge/files directory with a bunch of files starting with pwncollege. Tab-complete from /challenge/files/p or so, and make your way to the flag!


### Solve
**Flag: pwn.college{8fYdR0K5-4L17KXZekvYGoaJzOi.0lN0EzNxwyNwAzNzEzW}**

```bash
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwn
pwn                    pwn-the-planet         pwncollege-flag        pwncollege-flyswatter
pwn-college            pwncollege-family      pwncollege-flamingo    pwncollege-hacking
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwncollege-
pwncollege-family      pwncollege-flag        pwncollege-flamingo    pwncollege-flyswatter  pwncollege-hacking
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwncollege-fl
pwncollege-flag        pwncollege-flamingo    pwncollege-flyswatter
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwncollege-flag
pwn.college{8fYdR0K5-4L17KXZekvYGoaJzOi.0lN0EzNxwyNwAzNzEzW}
```

First after /challenge/files I wrote, pwn, pressed tab to show more files beginning with pwn, noticing pwncollege, therefore narrowed it down to pwncollege-fl, then pressed tab once again to find its list containing pwncollege-flag, then using cat, printed its contents to obtain the flag 

### New Learnings
How 1st tab can help auto-complete and 2nd tab can print the list of options in tab-completion.

## Tab completion on commands 
Tab completion is for more than files! You can also tab-complete commands. This level has a command that starts with pwncollege, and it'll give you the flag. Type pwncollege and hit the tab key to auto-complete it!

NOTE: You can auto-complete any command, but be careful: callous auto-completes without double-checking the result can wreak havoc in your shell if you accidentally run the wrong commands!

### Solve 
**Flag: pwn.college{of7Mzk3gIs26d-iYAm4erWQ39Eo.0VN0EzNxwyNwAzNzEzW}**

```bash
hacker@globbing~tab-completion-on-commands:~$ pwncollege-23642
Correct! Here is your flag:
pwn.college{of7Mzk3gIs26d-iYAm4erWQ39Eo.0VN0EzNxwyNwAzNzEzW}
```

I typed out pwncollege and pressed tab to reveal a set of digits 23642 which when used as a command with pwncollege, prints the flag

### New Learnings 
Better understanding of how tab-completion works and that it can also work on commands 