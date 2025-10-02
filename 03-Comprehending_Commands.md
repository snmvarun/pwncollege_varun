# Comprehending Commands 

## cat: not the pet, but the command 
One of the most critical Linux commands is cat. cat is most often used for reading out files, like so:
```bash
hacker@dojo:~$ cat /challenge/DESCRIPTION.md
One of the most critical Linux commands is `cat`.
`cat` is most often used for reading out files, like so:
```
cat will concatenate (hence the name) multiple files if provided multiple arguments. For example:
```bash
hacker@dojo:~$ cat myfile
This is my file!
hacker@dojo:~$ cat yourfile
This is your file!
hacker@dojo:~$ cat myfile yourfile
This is my file!
This is your file!
hacker@dojo:~$ cat myfile yourfile myfile
This is my file!
This is your file!
This is my file!
Finally, if you give no arguments at all, cat will read from the terminal input and output it. We'll explore that in later challenges...
```
In this challenge, I will copy the flag to the flag file in your home directory (where your shell starts). Go read it with cat!

### Solve 
**Flag: pwn.college{MZaSaCKd8dCzg27Xt3n369yHtf4.QXxcTN0wyNwAzNzEzW}**

```bash
hacker@commands~cat-not-the-pet-but-the-command:~$ cat flag
pwn.college{MZaSaCKd8dCzg27Xt3n369yHtf4.QXxcTN0wyNwAzNzEzW}
```
cat command with the flag 
### New learnings
Learnt how to use the cat command

## Catting absolute paths
In the last level, you did cat flag to read the flag out of your home directory! You can, of course, specify cat's arguments as absolute paths:
```bash
hacker@dojo:~$ cat /challenge/DESCRIPTION.md

In the last level, you did `cat flag` to read the flag out of your home directory!
You can, of course, specify `cat`'s arguments as absolute paths:
...
```
In this directory, I will not copy it to your home directory, but I will make it readable. You can read it with cat at its absolute path: /flag.

FUN FACT: /flag is where the flag always lives in pwn.college, but unlike in this challenge, you typically can't access that file directly.

### Solve 
**Flag: pwn.college{c31FK4PhRhyROo8CEDPlMoGhP71.QX5ETO0wyNwAzNzEzW}**

```bash
hacker@commands~catting-absolute-paths:~$ cat /flag
pwn.college{c31FK4PhRhyROo8CEDPlMoGhP71.QX5ETO0wyNwAzNzEzW}
```
absolute path of flag

### New learnings
How to read with cat at its absolute path

## more catting practice

You can specify all sorts of paths as arguments to commands, and we'll practice some more with cat. In this level, I'll put the flag in some crazy directory, and I will not allow you to change directories with cd, so no cat flag for you. You must retrieve the flag by absolute path, wherever it is.

### Solve
```bash
hacker@commands~more-catting-practice:~$ cat /lib/locale/C.UTF-8/flag
pwn.college{wIHaQv625VwWaSJbByeY98u5g3Z.QXwITO0wyNwAzNzEzW}
```

### New learnings
how to retrieve the flag by absolute path

##  grepping for a needle in a haystack
Sometimes, the files that you might cat out are too big. Luckily, we have the grep command to search for the contents we need! We'll learn it in this challenge.

There are many ways to grep, and we'll learn one way here:
```bash
hacker@dojo:~$ grep SEARCH_STRING /path/to/file
```
Invoked like this, grep will search the file for lines of text containing SEARCH_STRING and print them to the console.

In this challenge, I've put a hundred thousand lines of text into the /challenge/data.txt file. grep it for the flag!

HINT: The flag always starts with the text pwn.college.
### Solve
**Flag: pwn.college{w7iwllsMFdxE_ZB5KDe5OftKtwQ.QX3EDO0wyNwAzNzEzW}**

```bash
hacker@commands~grepping-for-a-needle-in-a-haystack:~$ grep pwn.college /challenge/data.txt
pwn.college{w7iwllsMFdxE_ZB5KDe5OftKtwQ.QX3EDO0wyNwAzNzEzW}
```
grepped a string beginning with pwn.college in the /challenge/data.txt texts

### New learnings 
how to use the grep command to search in a big file

## comparing files
When looking for changes between similar files, eyeballing them might not be the most efficient approach! This is where the diff command becomes invaluable.

diff compares two files line by line and shows you exactly what's different between them. For example:
```bash
hacker@dojo:~$ cat file1
hello
world
hacker@dojo:~$ cat file2
hello
universe
hacker@dojo:~$ diff file1 file2
2c2
< world
---
> universe
```
The output tells us that line 2 changed (2c2), with world in the first file (<) being replaced by universe in the second file (>).

Sometimes, when new lines are added, you'll see something like:
```bash
hacker@dojo:~$ cat old
pwn
hacker@dojo:~$ cat new
pwn
college
hacker@dojo:~$ diff old new
1a2
> college
```
This tells us that after line 1 in the first file, the second file has an additional line (1a2 means "after line 1 of file1, add line 2 of file2").

Now for your challenge! There are two files in /challenge:

/challenge/decoys_only.txt contains 100 fake flags
/challenge/decoys_and_real.txt contains all 100 fake flags plus the one real flag
Use diff to find what's different between these files and get your flag!

## Solve
**Flag: pwn.college{MIeBI9nb-W6Whnl3ON-HKq9Okum.01MwMDOxwyNwAzNzEzW}**

```bash
hacker@commands~comparing-files:~$ diff /challenge/decoys_only.txt /challenge/decoys_and_real.txt
70a71
> pwn.college{MIeBI9nb-W6Whnl3ON-HKq9Okum.01MwMDOxwyNwAzNzEzW}
```

difference between txt1 and txt2 is the one real flag, therefore when difference is taken, the real flag remains 

### New Learnings
diff command and its functions

## listing files
So far, we've told you which files to interact with. But directories can have lots of files (and other directories) inside them, and we won't always be here to tell you their names. You'll need to learn to list their contents using the ls command!

ls will list files in all the directories provided to it as arguments, and in the current directory if no arguments are provided. Observe:
```bash
hacker@dojo:~$ ls /challenge
run
hacker@dojo:~$ ls
Desktop    Downloads  Pictures  Templates
Documents  Music      Public    Videos
hacker@dojo:~$ ls /home/hacker
Desktop    Downloads  Pictures  Templates
Documents  Music      Public    Videos
hacker@dojo:~$
```

In this challenge, we've named /challenge/run with some random name! List the files in /challenge to find it

### Solve
**Flag: pwn.college{wwDD_KWkTMgKn-qMaieoH_Ot2mw.QX4IDO0wyNwAzNzEzW}**

```bash
hacker@commands~listing-files:~$ ls /challenge
13985-renamed-run-9057  DESCRIPTION.md
hacker@commands~listing-files:~$ /challenge/13985-renamed-run-9057
Yahaha, you found me! Here is your flag:
pwn.college{wwDD_KWkTMgKn-qMaieoH_Ot2mw.QX4IDO0wyNwAzNzEzW}
```
using ls to find the hidden renamed file in challenge and executing the absolute path to find the flag

### New Learnings 
the use of ls command to list

## touching files
Of course, you can also create files! There are several ways to do this, but we'll look at a simple command here. You can create a new, blank file by touching it with the touch command:
```bash
hacker@dojo:~$ cd /tmp
hacker@dojo:/tmp$ ls
hacker@dojo:/tmp$ touch pwnfile
hacker@dojo:/tmp$ ls
pwnfile
hacker@dojo:/tmp$
```
It's that simple! In this level, please create two files: /tmp/pwn and /tmp/college, and run /challenge/run to get your flag!

### Solve
**Flag: pwn.college{wbdc0BVMO7o9x0ZUzhM_FsNgE7q.QXwMDO0wyNwAzNzEzW}**

```bash
hacker@commands~touching-files:~$ cd /tmp
hacker@commands~touching-files:/tmp$ touch pwn
hacker@commands~touching-files:/tmp$ touch college
hacker@commands~touching-files:/tmp$ ls
bin  college  hsperfdata_root  pwn  tmp.TpSOPGOVKK
hacker@commands~touching-files:/tmp$ /challenge/run
Success! Here is your flag:
pwn.college{wbdc0BVMO7o9x0ZUzhM_FsNgE7q.QXwMDO0wyNwAzNzEzW}
```
To solve this I changed my directory to /tmp, created two pwn and college files by using touch command and then checked the listing, finally running /challenge/run to obtain my flag

### New Learnings
How to use the touch command to create files

## Removing files
Files are all around you. Like candy wrappers, there'll eventually be too many of them. In this level, we'll learn to clean up!

In Linux, you remove files with the rm command, as so:
```bash
hacker@dojo:~$ touch PWN
hacker@dojo:~$ touch COLLEGE
hacker@dojo:~$ ls
COLLEGE     PWN
hacker@dojo:~$ rm PWN
hacker@dojo:~$ ls
COLLEGE
hacker@dojo:~$
```
Let's practice. This challenge will create a delete_me file in your home directory! Delete it, then run /challenge/check, which will make sure you've deleted it and then give you the flag!

### Solve 
**Flag: pwn.college{E9QCdr0qc9rHlcLPkU4AllCe4nX.QX2kDM1wyNwAzNzEzW}**

```bash
hacker@commands~removing-files:~$ ls
Desktop  delete_me  key  key.pub  r
hacker@commands~removing-files:~$ rm delete_me
hacker@commands~removing-files:~$ ls
Desktop  key  key.pub  r
hacker@commands~removing-files:~$ /challenge/check
Excellent removal. Here is your reward:
pwn.college{E9QCdr0qc9rHlcLPkU4AllCe4nX.QX2kDM1wyNwAzNzEzW}
```
checked the listing to check what to delete, deleted delete_me using rm command and ran /challenge/check as instructed to get the flag

### New Learnings
how to use the rm command 

## moving files
You can also move files around with the mv command. The usage is simple:
```bash
hacker@dojo:~$ ls
my-file
hacker@dojo:~$ cat my-file
PWN!
hacker@dojo:~$ mv my-file your-file
hacker@dojo:~$ ls
your-file
hacker@dojo:~$ cat your-file
PWN!
hacker@dojo:~$
```
This challenge wants you to move the /flag file into /tmp/hack-the-planet (do it)! When you're done, run /challenge/check, which will check things out and give the flag to you.

### Solve
**Flag: pwn.college{oStfLvZSW4S2tCV8OGUrWasWvw8.0VOxEzNxwyNwAzNzEzW}**

```bash
hacker@commands~moving-files:~$ mv /flag /tmp/hack-the-planet
Correct! Performing 'mv /flag /tmp/hack-the-planet'.
hacker@commands~moving-files:~$ /challenge/check
Congrats! You successfully moved the flag to /tmp/hack-the-planet! Here it is:
pwn.college{oStfLvZSW4S2tCV8OGUrWasWvw8.0VOxEzNxwyNwAzNzEzW}
```
moved /flag file into /tmp/hack-the-planet using the mv command and then executed /challenge/check to obtain the flag

### New Learnings
How to use the mv command

## hidden files
Interestingly, ls doesn't list all the files by default. Linux has a convention where files that start with a . don't show up by default in ls and in a few other contexts. To view them with ls, you need to invoke ls with the -a flag, as so:
```bash
hacker@dojo:~$ touch pwn
hacker@dojo:~$ touch .college
hacker@dojo:~$ ls
pwn
hacker@dojo:~$ ls -a
.college	pwn
hacker@dojo:~$
Now, it's your turn! Go find the flag, hidden as a dot-prepended file in /.
```
### Solve
**Flag: pwn.college{g0A_ehhKrObkGE6d585IZKD1MBP.QXwUDO0wyNwAzNzEzW}** 

```bash
hacker@commands~hidden-files:~$ cd /
hacker@commands~hidden-files:/$ ls
bin   challenge  etc   lib    lib64   media  nix  proc  run   srv  tmp  var
boot  dev        home  lib32  libx32  mnt    opt  root  sbin  sys  usr
hacker@commands~hidden-files:/$ ls -a
.   .dockerenv            bin   challenge  etc   lib    lib64   media  nix  proc  run   srv  tmp  var
..  .flag-53051436229066  boot  dev        home  lib32  libx32  mnt    opt  root  sbin  sys  usr 
hacker@commands~hidden-files:/$ cat .flag-53051436229066
pwn.college{g0A_ehhKrObkGE6d585IZKD1MBP.QXwUDO0wyNwAzNzEzW}
```
First changing my directory to /, I checked its listing to find no flag as it started with ., so using ls -a to find the hidden files and then using the cat command to display the contents of the flag file, printing the flag.

### New Learnings 
How to check hidden files by involing ls using the -a flag.

## An Epic Filesystem Quest 
With your knowledge of cd, ls, and cat, we're ready to play a little game!

We'll start it out in /. Normally:
```bash
hacker@dojo:~$ cd /
hacker@dojo:/$ ls
bin   challenge  etc   home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  dev        flag  lib   lib64  media   opt  root  sbin  sys  usr
```
That's a lot of contents! One day, you will be quite familiar with them, but already, you might recognize the flag file and the challenge directory.

In this challenge, I have hidden the flag! Here, you will use ls and cat to follow my breadcrumbs and find it! Here's how it'll work:

0. Your first clue is in /. Head on over there.
1. Look around with ls. There'll be a file named HINT or CLUE or something along those lines!
2. cat that file to read the clue!
3. Depending on what the clue says, head on over to the next directory (or don't!).
4. Follow the clues to the flag!
Good luck!

### Solve 
**Flag: pwn.college{QLQfDDnwqH3Cf4MTNFGg6IWfpQY.QX5IDO0wyNwAzNzEzW}**
```bash
hacker@commands~an-epic-filesystem-quest:~$ cd /
hacker@commands~an-epic-filesystem-quest:/$ ls
CUE  boot       dev  flag  lib    lib64   media  nix  proc  run   srv  tmp  var
bin  challenge  etc  home  lib32  libx32  mnt    opt  root  sbin  sys  usr
hacker@commands~an-epic-filesystem-quest:/$ cat CUE
Yahaha, you found me!
The next clue is in: /opt/linux/linux-5.4/arch/xtensa/variants/de212/include

The next clue is **delayed** --- it will not become readable until you enter the directory with 'cd'.
hacker@commands~an-epic-filesystem-quest:/$ cd /opt/linux/linux-5.4/arch/xtensa/variants/de212/include
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/arch/xtensa/variants/de212/include$ cat CUE
cat: CUE: No such file or directory
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/arch/xtensa/variants/de212/include$ ls
DISPATCH  variant
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/arch/xtensa/variants/de212/include$ cat DISPATCH
Lucky listing!
The next clue is in: /opt/linux/linux-5.4/drivers/tee/optee

The next clue is **delayed** --- it will not become readable until you enter the directory with 'cd'.
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/arch/xtensa/variants/de212/include$ cd /opt/linux/linux-5.4/drivers/tee/optee
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/drivers/tee/optee$ ls
DOSSIER  Makefile  core.c    optee_msg.h      optee_smc.h  shm_pool.c  supp.c
Kconfig  call.c    device.c  optee_private.h  rpc.c        shm_pool.h
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/drivers/tee/optee$ cat DOSSIER
Lucky listing!
The next clue is in: /usr/share/doc/emacs-common

Watch out! The next clue is **trapped**. You'll need to read it out without 'cd'ing into the directory; otherwise, the clue will self destruct!
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/drivers/tee/optee$ ls /usr/share/doc/emacs-common
BRIEF-TRAPPED  BUGS  README.Debian.gz  README.add-on-package-maintainers  README.gz  changelog.Debian.gz  copyright
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/drivers/tee/optee$ cat /usr/share/doc/emacs-common/BRIEF-TRAPPED
Tubular find!
The next clue is in: /usr/lib/ruby/vendor_ruby/power_assert

The next clue is **hidden** --- its filename starts with a '.' character. You'll need to look for it using special options to 'ls'.
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/drivers/tee/optee$ cd /usr/lib/ruby/vendor_ruby/power_assert
hacker@commands~an-epic-filesystem-quest:/usr/lib/ruby/vendor_ruby/power_assert$ ls -a
.  ..  .MESSAGE  colorize.rb  configuration.rb  context.rb  enable_tracepoint_events.rb  inspector.rb  parser.rb  version.rb
hacker@commands~an-epic-filesystem-quest:/usr/lib/ruby/vendor_ruby/power_assert$ cat .MESSAGE
Lucky listing!
The next clue is in: /usr/share/doc/libkrb5-3

The next clue is **delayed** --- it will not become readable until you enter the directory with 'cd'.
hacker@commands~an-epic-filesystem-quest:/usr/lib/ruby/vendor_ruby/power_assert$ cd /usr/share/doc/libkrb5-3
hacker@commands~an-epic-filesystem-quest:/usr/share/doc/libkrb5-3$ ls
EVIDENCE  README.Debian  README.gz  changelog.Debian.gz  copyright
hacker@commands~an-epic-filesystem-quest:/usr/share/doc/libkrb5-3$ cat EVIDENCE
Great sleuthing!
The next clue is in: /usr/share/racket/pkgs/mzscheme-lib/compiler
hacker@commands~an-epic-filesystem-quest:/usr/share/doc/libkrb5-3$ cd /usr/share/racket/pkgs/mzscheme-lib/compiler
hacker@commands~an-epic-filesystem-quest:/usr/share/racket/pkgs/mzscheme-lib/compiler$ ls
INFO  compiled  info.rkt  main.rkt  mzc.1
hacker@commands~an-epic-filesystem-quest:/usr/share/racket/pkgs/mzscheme-lib/compiler$ cat INFO
Great sleuthing!
The next clue is in: /usr/local/lib/python3.8/dist-packages/selenium/webdriver/chrome/__pycache__
hacker@commands~an-epic-filesystem-quest:/usr/share/racket/pkgs/mzscheme-lib/compiler$ cd /usr/local/lib/python3.8/dist-packages/selenium/webdriver/chrome/__pycache__
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/selenium/webdriver/chrome/__pycache__$ ls
GIST                     options.cpython-38.pyc            service.cpython-38.pyc
__init__.cpython-38.pyc  remote_connection.cpython-38.pyc  webdriver.cpython-38.pyc
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/selenium/webdriver/chrome/__pycache__$ cat GIST
Yahaha, you found me!
The next clue is in: /opt/linux/linux-5.4/drivers/clk/x86

The next clue is **hidden** --- its filename starts with a '.' character. You'll need to look for it using special options to 'ls'.
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/selenium/webdriver/chrome/__pycache__$ cd /opt/linux/linux-5.4/drivers/clk/x86
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/drivers/clk/x86$ ls -a
.  ..  .SPOILER  .built-in.a.cmd  .clk-pmc-atom.o.cmd  Makefile  built-in.a  clk-lpt.c  clk-pmc-atom.c  clk-pmc-atom.o  clk-st.c
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/drivers/clk/x86$ cat .SPOILER
CONGRATULATIONS! Your perserverence has paid off, and you have found the flag!
It is: pwn.college{QLQfDDnwqH3Cf4MTNFGg6IWfpQY.QX5IDO0wyNwAzNzEzW}
```
I changed directories and printed the file items to constantly keep checking through the files by the conditions provided, finally printing the flag

### New Learnings
Moving through multiple directories and files and displaying/printing file contents.

## making directories
We can create files. How about directories? You make directories using the mkdir command. Then you can stick files in there!

Watch:
```bash
hacker@dojo:~$ cd /tmp
hacker@dojo:/tmp$ ls
hacker@dojo:/tmp$ ls
hacker@dojo:/tmp$ mkdir my_directory
hacker@dojo:/tmp$ ls
my_directory
hacker@dojo:/tmp$ cd my_directory
hacker@dojo:/tmp/my_directory$ touch my_file
hacker@dojo:/tmp/my_directory$ ls
my_file
hacker@dojo:/tmp/my_directory$ ls /tmp/my_directory/my_file
/tmp/my_directory/my_file
hacker@dojo:/tmp/my_directory$
```
Now, go forth and create a /tmp/pwn directory and make a college file in it! Then run /challenge/run, which will check your solution and give you the flag!

### Solve
**Flag: pwn.college{gdi2oHfCfxgDPremQbDZrqxRBOh.QXxMDO0wyNwAzNzEzW}**

```bash
hacker@commands~making-directories:~$ cd /tmp
hacker@commands~making-directories:/tmp$ mkdir pwn
hacker@commands~making-directories:/tmp$ cd pwn
hacker@commands~making-directories:/tmp/pwn$ touch college
hacker@commands~making-directories:/tmp/pwn$ /challenge/run
Success! Here is your flag:
pwn.college{gdi2oHfCfxgDPremQbDZrqxRBOh.QXxMDO0wyNwAzNzEzW}
```
First changing directory to /tmp, I made directory 'pwn' in /tmp, then changing the directory to pwn which I made, I made a file called college, then ran the /challenge/run and obtained my flag.

### New learnings
How to make a directory.

## finding files
So now we know how to list, read, and create files. But how do we find them? We use the find command!

The find command takes optional arguments describing the search criteria and the search location. If you don't specify a search criteria, find matches every file. If you don't specify a search location, find uses the current working directory (.). For example:
```bash
hacker@dojo:~$ mkdir my_directory
hacker@dojo:~$ mkdir my_directory/my_subdirectory
hacker@dojo:~$ touch my_directory/my_file
hacker@dojo:~$ touch my_directory/my_subdirectory/my_subfile
hacker@dojo:~$ find
.
./my_directory
./my_directory/my_subdirectory
./my_directory/my_subdirectory/my_subfile
./my_directory/my_file
hacker@dojo:~$
```
And when specifying the search location:
```bash
hacker@dojo:~$ find my_directory/my_subdirectory
my_directory/my_subdirectory
my_directory/my_subdirectory/my_subfile
hacker@dojo:~$
```
And, of course, we can specify the criteria! For example, here, we filter by name:
```bash
hacker@dojo:~$ find -name my_subfile
./my_directory/my_subdirectory/my_subfile
hacker@dojo:~$ find -name my_subdirectory
./my_directory/my_subdirectory
hacker@dojo:~$
```
You can search the whole filesystem if you want!
```bash
hacker@dojo:~$ find / -name hacker
/home/hacker
hacker@dojo:~$
```
Now it's your turn. I've hidden the flag in a random directory on the filesystem. It's still called flag. Go find it!

Several notes. First, there are other files named flag on the filesystem. Don't panic if the first one you try doesn't have the actual flag in it. Second, there're plenty of places in the filesystem that are not accessible to a normal user. These will cause find to generate errors, but you can ignore those; we won't hide the flag there! Finally, find can take a while; be patient!

### Solve
**Flag: pwn.college{oenJehOmJPs1WMG_vczNzrbrJEG.QXyMDO0wyNwAzNzEzW}**

```bash 
hacker@commands~finding-files:~$ find / -name flag
find: ‘/root’: Permission denied
find: ‘/etc/ssl/private’: Permission denied
find: ‘/tmp/tmp.TpSOPGOVKK’: Permission denied
/usr/local/lib/python3.8/dist-packages/pwnlib/flag
find: ‘/var/cache/apt/archives/partial’: Permission denied
find: ‘/var/cache/ldconfig’: Permission denied
find: ‘/var/cache/private’: Permission denied
find: ‘/var/log/private’: Permission denied
find: ‘/var/log/apache2’: Permission denied
find: ‘/var/log/mysql’: Permission denied
find: ‘/var/lib/apt/lists/partial’: Permission denied
find: ‘/var/lib/mysql-keyring’: Permission denied
find: ‘/var/lib/php/sessions’: Permission denied
find: ‘/var/lib/private’: Permission denied
find: ‘/var/lib/mysql-files’: Permission denied
find: ‘/var/lib/mysql’: Permission denied
find: ‘/run/mysqld’: Permission denied
find: ‘/run/sudo’: Permission denied
find: ‘/proc/tty/driver’: Permission denied
find: ‘/proc/1/task/1/fd’: Permission denied
find: ‘/proc/1/task/1/fdinfo’: Permission denied
find: ‘/proc/1/task/1/ns’: Permission denied
find: ‘/proc/1/fd’: Permission denied
find: ‘/proc/1/map_files’: Permission denied
find: ‘/proc/1/fdinfo’: Permission denied
find: ‘/proc/1/ns’: Permission denied
find: ‘/proc/7/task/7/fd’: Permission denied
find: ‘/proc/7/task/7/fdinfo’: Permission denied
find: ‘/proc/7/task/7/ns’: Permission denied
find: ‘/proc/7/fd’: Permission denied
find: ‘/proc/7/map_files’: Permission denied
find: ‘/proc/7/fdinfo’: Permission denied
find: ‘/proc/7/ns’: Permission denied
/opt/linux/linux-5.4/arch/x86/boot/compressed/flag
/opt/pwndbg/.venv/lib/python3.8/site-packages/pwnlib/flag
/nix/store/7ns27apnvn4qj4q5c82x0z1lzixrz47p-radare2-5.9.8/share/radare2/5.9.8/flag
/nix/store/5z3sjp9r463i3siif58hq5wj5jmy5m98-python3.12-pwntools-4.13.1/lib/python3.12/site-packages/pwnlib/flag
/nix/store/5n5lp1m8gilgrsriv1f2z0jdjk50ypcn-rizin-0.7.3/share/rizin/flag
/nix/store/h88mxp2mbgyj06vypwmqpy05idhwimnp-python3.13-pwntools-4.14.1/lib/python3.13/site-packages/pwnlib/flag
/nix/store/s8b49lb0pqwvw0c6kgjbxdwxcv2bp0x4-radare2-5.9.8/share/radare2/5.9.8/flag
/nix/store/bnlabj2vsbljhp597ir29l51nrqhm89w-rizin-0.7.4/share/rizin/flag
/nix/store/1hyxipvwpdpcxw90l5pq1nvd6s6jdi5m-python3.12-pwntools-4.14.1/lib/python3.12/site-packages/pwnlib/flag
/nix/store/5qz6hgb1qzpvjrsw20wyiylx5zw8b9bk-pwntools-4.14.0/lib/python3.13/site-packages/pwnlib/flag
hacker@commands~finding-files:~$ cd /usr/local/lib/python3.8/dist-packages/pwnlib/
hacker@commands~finding-files:/usr/local/lib/python3.8/dist-packages/pwnlib$ cat flag
cat: flag: Is a directory
hacker@commands~finding-files:/usr/local/lib/python3.8/dist-packages/pwnlib$ cd
hacker@commands~finding-files:~$ cd /opt/linux/linux-5.4/arch/x86/boot/compressed/
hacker@commands~finding-files:/opt/linux/linux-5.4/arch/x86/boot/compressed$ cat flag
pwn.college{oenJehOmJPs1WMG_vczNzrbrJEG.QXyMDO0wyNwAzNzEzW}
```
Searching through the given files by the find command, to find the flag

### New Learnings
How to use the find command to find files

## linking files
If you use Linux (or computers) for any reasonable length of time to do any real work, you will eventually run into some variant of the following situation: you want two programs to access the same data, but the programs expect that data to be in two different locations. Luckily, Linux provides a solution to this quandary: links.

Links come in two flavors: hard and soft (also known as symbolic) links. We'll differentiate the two with an analogy:

A hard link is when you address your apartment using multiple addresses that all lead directly to the same place (e.g., Apt 2 vs Unit 2).
A soft link is when you move apartments and have the postal service automatically forward your mail from your old place to your new place.
In a filesystem, a file is, conceptually, an address at which the contents of that file live. A hard link is an alternate address that indexes that data --- accesses to the hard link and accesses to the original file are completely identical, in that they immediately yield the necessary data. A soft/symbolic link, instead, contains the original file name. When you access the symbolic link, Linux will realize that it is a symbolic link, read the original file name, and then (typically) automatically access that file. In most cases, both situations result in accessing the original data, but the mechanisms are different.

Hard links sound simpler to most people (case in point, I explained it in one sentence above, versus two for soft links), but they have various downsides and implementation gotchas that make soft/symbolic links, by far, the more popular alternative.

In this challenge, we will learn about symbolic links (also known as symlinks). Symbolic links are created with the ln command with the -s argument, like so:
```bash
hacker@dojo:~$ cat /tmp/myfile
This is my file!
hacker@dojo:~$ ln -s /tmp/myfile /home/hacker/ourfile
hacker@dojo:~$ cat ~/ourfile
This is my file!
hacker@dojo:~$
```
You can see that accessing the symlink results in getting the original file contents! Also, you can see the usage of ln -s. Note that the original file path comes before the link path in the command!

A symlink can be identified as such with a few methods. For example, the file command, which takes a filename and tells you what type of file it is, will recognize symlinks:
```bash
hacker@dojo:~$ file /tmp/myfile
/tmp/myfile: ASCII text
hacker@dojo:~$ file ~/ourfile
/home/hacker/ourfile: symbolic link to /tmp/myfile
hacker@dojo:~$
```
Okay, now you try it! In this level the flag is, as always, in /flag, but /challenge/catflag will instead read out /home/hacker/not-the-flag. Use the symlink, and fool it into giving you the flag!

### Solve 
**Flag: pwn.college{MJl9Yh-Zbu4ATpFVMjtsiqYK26e.QX5ETN1wyNwAzNzEzW}**

```bash
hacker@commands~linking-files:~$ ln -s /flag ~/not-the-flag
hacker@commands~linking-files:~$ /challenge/catflag
About to read out the /home/hacker/not-the-flag file!
pwn.college{MJl9Yh-Zbu4ATpFVMjtsiqYK26e.QX5ETN1wyNwAzNzEzW}
```
linked /flag to /home/hacker/not-the-flag, and then executed /challenge/flag to obtain the flag

### New Learnings 
How to link files together using symbolic links 
