# Untangling Users

## Becoming root with su
It's not just hackers that need to become root. Oftentimes, you, as the owner of your computer, need to use root access to administer it. Becoming root is a fairly common action that Linux users take, and there are two utilities that exist for this purpose: su and sudo.

In this challenge, we will cover the older one, su (the substitute user command). This is not typically used to elevate to root access anymore, but it is an elegant utility from a more civilized time, and we'll cover it first.

su is a setuid binary:
```bash
hacker@dojo:~$ ls -l /usr/bin/su
-rwsr-xr-x 1 root root 232416 Dec 1 11:45 /usr/bin/su
hacker@dojo:~$
```

Because it has the SUID bit set, su runs as root. Running as root, it can start a root shell! Of course, su is discerning: before allowing the user to elevate privileges to root, it checks to make sure that the user knows the root password:
```bash
hacker@dojo:~$ su
Password: 
su: Authentication failure
hacker@dojo:~$
```
This check against the root password is what obsoletes su. Modern systems very rarely have root passwords, and different mechanisms (that we will learn later) are used to grant administrative access.

But THIS challenge (and only this challenge) does have a root password. That password is hack-the-planet, and you must provide it to su to become root! Go do that, and read the flag!

### Solve
**Flag: pwn.college{gdBJKiDw0AkyGuJAHwTIJCff7qt.QX1UDN1wyNwAzNzEzW} **

```bash
hacker@users~becoming-root-with-su:~$ su
Password:
root@users~becoming-root-with-su:/home/hacker# ls
COLLEGE  PWN  college       key      log     myflag        output.log  r     the-flag
Desktop  cm   instructions  key.pub  mkfifo  not-the-flag  pwn         temp
root@users~becoming-root-with-su:/home/hacker# cat myflag

[FLAG] Here is your flag:
[FLAG] pwn.college{swsZ3sbOMykZBenSst57b5wl6Kp.QX3YTN0wyNwAzNzEzW}

root@users~becoming-root-with-su:/home/hacker# cat not-the-flag
pwn.college{gdBJKiDw0AkyGuJAHwTIJCff7qt.QX1UDN1wyNwAzNzEzW} 
```
I ran the su command to access administration, then I entered the password hack-the-planet, then checked my listing and looked through multiple flags to search for the correct one until not-the-flag contained the right flag.

### New Learnings
How to use the su to access administration.

## Other users with su
With no arguments, su will start a root shell (after authenticating with root's password). However, you can also give a username as an argument to switch to that user instead of root. For example:
```bash
hacker@dojo:~$ su some-user
Password:
some-user@dojo:~$
```
Awesome! In this level, you must switch to the zardus user and then run /challenge/run. Zardus' password is dont-hack-me. Good luck!

### Solve
**Flag: pwn.college{IUfo8Mo7wBeOlhvLn6UfisTyjIG.QX2UDN1wyNwAzNzEzW}**

```bash
hacker@users~other-users-with-su:~$ su zardus
Password:
zardus@users~other-users-with-su:/home/hacker$ /challenge/run
Congratulations, you have become Zardus! Here is your flag:
pwn.college{IUfo8Mo7wBeOlhvLn6UfisTyjIG.QX2UDN1wyNwAzNzEzW}
```
I changed my user to zardus using su command and zardus as its argument, and then entered it's password as "dont-hack-me" and then ran /challenge/run from the zardus user to obtain the flag.

### New Learnings
How to use the su command to change users.

## Cracking passwords
When you enter a password for su, it compares it against the stored password for that user. These passwords used to be stored in /etc/passwd, but because /etc/passwd is a globally-readable file, this is not good for passwords, these were moved to /etc/shadow. Here is the example /etc/shadow from the previous level:
```bash
root:$6$s74oZg/4.RnUvwo2$hRmCHZ9rxX56BbjnXcxa0MdOsW2moiW8qcAl/Aoc7NEuXl2DmJXPi3gLp7hmyloQvRhjXJ.wjqJ7PprVKLDtg/:19921:0:99999:7:::
daemon:*:19873:0:99999:7:::
bin:*:19873:0:99999:7:::
sys:*:19873:0:99999:7:::
sync:*:19873:0:99999:7:::
games:*:19873:0:99999:7:::
man:*:19873:0:99999:7:::
lp:*:19873:0:99999:7:::
mail:*:19873:0:99999:7:::
news:*:19873:0:99999:7:::
uucp:*:19873:0:99999:7:::
proxy:*:19873:0:99999:7:::
www-data:*:19873:0:99999:7:::
backup:*:19873:0:99999:7:::
list:*:19873:0:99999:7:::
irc:*:19873:0:99999:7:::
gnats:*:19873:0:99999:7:::
nobody:*:19873:0:99999:7:::
_apt:*:19873:0:99999:7:::
systemd-timesync:*:19901:0:99999:7:::
systemd-network:*:19901:0:99999:7:::
systemd-resolve:*:19901:0:99999:7:::
mysql:!:19901:0:99999:7:::
messagebus:*:19901:0:99999:7:::
sshd:*:19901:0:99999:7:::
hacker::19916:0:99999:7:::
zardus:$6$bEFkpM0w/6J0n979$47ksu/JE5QK6hSeB7mmuvJyY05wVypMhMMnEPTIddNUb5R9KXgNTYRTm75VOu1oRLGLbAql3ylkVa5ExuPov1.:19921:0:99999:7:::
```

Separated by :s, the first field of each line is the username and the second is the password. A value of * or ! functionally means that password login for the account is disabled, a blank field means that there is no password (a not-uncommon misconfiguration that allows password-less su in some configurations), and the long string such as Zardus' $6$bEFkpM0w/6J0n979$47ksu/JE5QK6hSeB7mmuvJyY05wVypMhMMnEPTIddNUb5R9KXgNTYRTm75VOu1oRLGLbAql3ylkVa5ExuPov1. is the result of one-way-encrypting (hashing) Zardus' password from the last level (in this case, dont-hack-me). Other fields in this file have other meanings, and you can read more about them here.

When you input a password into su, it one-way-encrypts (hashes) it and compares the result against the stored value. If the result matches, su grants you access to the user!

But what if you don't know the password? If you have the hashed value of the password, you can crack it! Even though /etc/shadow is, by default, only readable by root, leaks can happen! For example, backups are often stored, unencrypted and insufficiently protected, on file servers, and this has led to countless data disclosures.

If a hacker gets their hands on a leaked /etc/shadow, they can start cracking passwords and wreaking havoc. The cracking can be done via the famous John the Ripper, as so:
```bash
hacker@dojo:~$ john ./my-leaked-shadow-file
Loaded 1 password hash (crypt, generic crypt(3) [?/64])
Will run 32 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
password1337      (zardus)
1g 0:00:00:22 3/3 0.04528g/s 10509p/s 10509c/s 10509C/s lykys..lank
Use the "--show" option to display all of the cracked passwords reliably
Session completed
hacker@dojo:~$
```

Here, John the Ripper cracked Zardus' leaked password hash to find the real value of password1337. Poor Zardus!

This level simulates this story, giving you a leak of /etc/shadow (in /challenge/shadow-leak). Crack it (this could take a few minutes), su to zardus, and run /challenge/run to get the flag!

### Solve
**Flag: pwn.college{wdwFedk_cEQ5vRgHr3pLzSbZ0M2.QX3UDN1wyNwAzNzEzW}**

```bash
hacker@users~cracking-passwords:~$ john /challenge/shadow-leak
Created directory: /home/hacker/.john
Loaded 1 password hash (crypt, generic crypt(3) [?/64])
Press 'q' or Ctrl-C to abort, almost any other key for status
aardvark         (zardus)
1g 0:00:00:20 100% 2/3 0.04906g/s 285.7p/s 285.7c/s 285.7C/s Johnson..buzz
Use the "--show" option to display all of the cracked passwords reliably
Session completed
hacker@users~cracking-passwords:~$ su zardus
Password:
zardus@users~cracking-passwords:/home/hacker$ /challenge/run
Congratulations, you have become Zardus! Here is your flag:
pwn.college{wdwFedk_cEQ5vRgHr3pLzSbZ0M2.QX3UDN1wyNwAzNzEzW}
```
I ran the john command with the file location of /challenge/shadow-leak which contains the list of passwords, I obtain the password of zardus user, and then change the user to zardus and then enter the obtained password to enter the account, and then ran /challenge/run and obtained the flag. 

### New Learnings
How to use the 'john' command to obtain and view cracked passwords.

## Using Sudo
In the olden days, a typical Linux system had a root password that administrators would use to su to root (after logging into their account with their normal account password). But root passwords are a pain to maintain, they (or their hashes!) can leak, and they don't lend themselves well to larger environments (e.g., fleets of servers). To address this, in recent decades, the world has moved from administration via su to administration via sudo (Fun Fact: sudo originally stood for superuser do, but has changed to "su 'do'", and because su stands for "substitute user", the current meaning of sudo is "substitute user, do").

Unlike su, which defaults to launching a shell as a specified user, sudo defaults to running a command as root:
```bash
hacker@dojo:~$ whoami
hacker
hacker@dojo:~$ sudo whoami
root
hacker@dojo:~$
```
Or, more relevant to getting flags:
```bash
hacker@dojo:~$ grep hacker /etc/shadow
grep: /etc/shadow: Permission denied
hacker@dojo:~$ sudo grep hacker /etc/shadow
hacker:$6$Xro.e7qB3Q2Jl2sA$j6xffIgWn9xIxWUeFzvwPf.nOH2NTWNJCU5XVkPuONjIC7jL467SR4bXjpVJx4b/bkbl7kyhNquWtkNlulFoy.:19921:0:99999:7:::
hacker@dojo:~$
```
Unlike su, which relies on password authentication, sudo checks policies to determine whether the user is authorized to run commands as root. These policies are defined in /etc/sudoers, and though it's mostly out of scale for our purposes, there are plenty of resources for learning about this!

So, the world has moved to sudo and has (for the purposes of system administration) left su behind. In fact, even pwn.college's Practice Mode works by giving you sudo access to elevate privileges!

In this level, we will give you sudo access, and you will use it to read the flag. Nice and easy!

NOTE: After this level, we will enable Privileged Mode! When you launch a challenge in Privileged Mode (by clicking the Privileged button instead of the Start button), the resulting container will give you full sudo access to allow you to introspect and debug to your heart's content, but of course with a placeholder flag.

### Solve
**Flag: pwn.college{UPTG8ysPbNYO3-kNIoO3cb5li_X.QX4UDN1wyNwAzNzEzW}**

```bash
hacker@users~using-sudo:~$ ls
COLLEGE  PWN  college       key      log     myflag        output.log  r     the-flag
Desktop  cm   instructions  key.pub  mkfifo  not-the-flag  pwn         temp
hacker@users~using-sudo:~$ cat not-the-flag
cat: not-the-flag: Permission denied
hacker@users~using-sudo:~$ sudo cat not-the-flag
pwn.college{UPTG8ysPbNYO3-kNIoO3cb5li_X.QX4UDN1wyNwAzNzEzW}
```
I checked the contents of directory, then I tried printing the contents of "not-the-flag" file to see that I do not have permissions, then I ran the same command with sudo to run it as administrator to find the flag.

### New Learnings
How to use the sudo command to run commands as an administrator.

