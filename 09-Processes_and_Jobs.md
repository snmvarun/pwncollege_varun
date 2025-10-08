# Processes and Jobs

## Listing Processes
First, we will learn to list running processes using the ps command. Depending on whom you ask, ps either stands for "process snapshot" or "process status", and it lists processes. By default, ps just lists the processes running in your terminal, which honestly isn't very useful:
```bash
hacker@dojo:~$ ps
    PID TTY          TIME CMD
    329 pts/0    00:00:00 bash
    349 pts/0    00:00:00 ps
hacker@dojo:~$
```

In the above example, we have the shell (bash) and the ps process itself, and that's all that's running on that specific terminal. We also see that each process has a numerical identifier (the Process ID, or PID), which is a number that uniquely identifies every running process in a Linux environment. We also see the terminal on which the commands are running (in this case, the designation pts/0), and the total amount of cpu time that the process has eaten up so far (since these processes are very undemanding, they have yet to eat up even 1 second!).

In the majority of cases, this is all that you'll see with a default ps. To make it useful, we need to pass a few arguments.

As ps is a very old utility, its usage is a bit of a mess. There are two ways to specify arguments.

"Standard" Syntax: in this syntax, you can use -e to list "every" process and -f for a "full format" output, including arguments. These can be combined into a single argument -ef.

"BSD" Syntax: in this syntax, you can use a to list processes for all users, x to list processes that aren't running in a terminal, and u for a "user-readable" output. These can be combined into a single argument aux.

These two methods, ps -ef and ps aux, result in slightly different, but cross-recognizable output.

Let's try it in the dojo:
```bash
hacker@dojo:~$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
hacker         1       0  0 05:34 ?        00:00:00 /sbin/docker-init -- /bin/sleep 6h
hacker         7       1  0 05:34 ?        00:00:00 /bin/sleep 6h
hacker       102       1  1 05:34 ?        00:00:00 /usr/lib/code-server/lib/node /usr/lib/code-server --auth=none -
hacker       138     102 11 05:34 ?        00:00:07 /usr/lib/code-server/lib/node /usr/lib/code-server/out/node/entr
hacker       287     138  0 05:34 ?        00:00:00 /usr/lib/code-server/lib/node /usr/lib/code-server/lib/vscode/ou
hacker       318     138  6 05:34 ?        00:00:03 /usr/lib/code-server/lib/node --dns-result-order=ipv4first /usr/
hacker       554     138  3 05:35 ?        00:00:00 /usr/lib/code-server/lib/node /usr/lib/code-server/lib/vscode/ou
hacker       571     554  0 05:35 pts/0    00:00:00 /usr/bin/bash --init-file /usr/lib/code-server/lib/vscode/out/vs
hacker       695     571  0 05:35 pts/0    00:00:00 ps -ef
hacker@dojo:~$
```
You can see here that there are processes running for the initialization of the challenge environment (docker-init), a timeout before the challenge is automatically terminated to preserve computing resources (sleep 6h to timeout after 6 hours), the VSCode environment (several code-server helper processes), the shell (bash), and my ps -ef command. It's basically the same thing with ps aux:
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
There are many commonalities between ps -ef and ps aux: both display the user (USER column), the PID, the TTY, the start time of the process (STIME/START), the total utilized CPU time (TIME), and the command (CMD/COMMAND). ps -ef additionally outputs the Parent Process ID (PPID), which is the PID of the process that launched the one in question, while ps aux outputs the percentage of total system CPU and Memory that the process is utilizing. Plus, there's a bunch of other stuff we won't get into right now.

Anyways! Let's practice. In this level, I have once again renamed /challenge/run to a random filename, and this time made it so that you cannot ls the /challenge directory! But I also launched it, so you can find it in the running process list, figure out the filename, and relaunch it directly for the flag! Good luck!

NOTE: Both ps -ef and ps aux truncate the command listing to the width of your terminal (which is why the examples above line up so nicely on the right side of the screen. If you can't read the whole path to the process, you might need to enlarge your terminal (or redirect the output somewhere to avoid this truncating behavior)! Alternatively, you can pass the w option twice (e.g., ps -efww or ps auxww) to disable the truncation.

### Solve
**Flag: pwn.college{4WDSFPquVDNSU3ALmqqXflWucLF.QX4MDO0wyNwAzNzEzW}**

```bash
hacker@processes~listing-processes:~$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 13:28 ?        00:00:00 /sbin/docker-init -- /nix/var/nix/profiles/dojo-workspace/bin/dojo-init /run/dojo
root           7       1  0 13:28 ?        00:00:00 /run/dojo/bin/sleep 6h
root         132       1  0 13:28 ?        00:00:00 /challenge/3705-run-13440
root         135     132  0 13:28 ?        00:00:00 sleep 6h
hacker       137       0  0 13:28 pts/0    00:00:00 /nix/store/0nxvi9r5ymdlr2p24rjj9qzyms72zld1-bash-interactive-5.2p37/bin/bash /run
hacker       143     137  0 13:28 pts/0    00:00:00 /run/dojo/bin/bash --login
hacker       153     143  0 13:33 pts/0    00:00:00 ps -ef
hacker@processes~listing-processes:~$ /challenge/3705-run-13440
Yahaha, you found me! Here is your flag:
pwn.college{4WDSFPquVDNSU3ALmqqXflWucLF.QX4MDO0wyNwAzNzEzW}
Now I will sleep for a while (so that you could find me with 'ps').
```
First I listed the processes by using the ps -ef command and then spotted the new name of /challenge/run to be /challenge/3705-run-13440, I ran it to find the flag

### New Learnings
How to use the ps command with the functions of its arguments: -ef and aux and the formats of the two types of process listings.


## Killing Processes 
You've launched processes, you've viewed processes, now you will learn to terminate processes! In Linux, this is done using the aggressively-named kill command. With default options (which is all we'll cover in this level), kill will terminate a process in a way that gives it a chance to get its affairs in order before ceasing to exist.

Let's say you had a pesky sleep process (sleep is a program that simply hangs out for the number of seconds specified on the commandline, in this case, 1337 seconds) that you launched in another terminal, like so:
```bash
hacker@dojo:~$ sleep 1337
```
How do we get rid of it? You use kill to terminate it by passing the process identifier (the PID from ps) as an argument, like so:
```bash
hacker@dojo:~$ ps -e | grep sleep
 342 pts/0    00:00:00 sleep
hacker@dojo:~$ kill 342
hacker@dojo:~$ ps -e | grep sleep
hacker@dojo:~$
```
Now, it's time to terminate your first process! In this challenge, /challenge/run will refuse to run while /challenge/dont_run is running! You must find the dont_run process and kill it. If you fail, pwn.college will disavow all knowledge of your mission. Good luck.

### Solve
**Flag: pwn.college{YXjhSAp0eQWMHwDRTZYZG04zUHv.QXyQDO0wyNwAzNzEzW}**

```bash
hacker@processes~killing-processes:~$ ps -ef | grep dont_run
hacker       136     135  0 13:39 ?        00:00:00 /challenge/dont_run
hacker       160     145  0 13:42 pts/0    00:00:00 grep --color=auto dont_run
hacker@processes~killing-processes:~$ kill 136
hacker@processes~killing-processes:~$ /challenge/run
Great job! Here is your payment:
pwn.college{YXjhSAp0eQWMHwDRTZYZG04zUHv.QXyQDO0wyNwAzNzEzW}
```
I ran the ps -ef to search full formats with the grep command piped searching for dont_run and list the process to find its PID, after spotting /challenge/run the second column gives the PID(third column gives the PPID) to be 136, I ran the kill command with 136 as the arguement to terminate it and then ran /challenge/run to get the flag.

### New Learnings
How to search for specific processes in process listing by piping the ps command with grep and how to use the kill command to terminate a process. 

## Interupting Processes 
You've learned how to kill other processes with the kill command, but sometimes you just want to get rid of the process that's clogging up your terminal! Luckily, terminals have a hotkey for this: Ctrl-C (e.g., holding down the Ctrl key and pressing C) sends an "interrupt" to whatever application is waiting on input from the terminal and, typically, this causes the application to cleanly exit.

Try it here! /challenge/run will refuse to give you the flag until you interrupt it. Good luck!

For the very interested, check out this article about terminals and "control codes" (such as Ctrl-C).

### Solve
**Flag: pwn.college{gqax2n0751nKwlMlyQ0fpEmqBXx.QXzQDO0wyNwAzNzEzW}**

```bash
hacker@processes~interrupting-processes:~$ /challenge/run
I could give you the flag... but I won't, until this process exits. Remember,
you can force me to exit with Ctrl-C. Try it now!
^C
Good job! You have used Ctrl-C to interrupt this process! Here is your flag:
pwn.college{gqax2n0751nKwlMlyQ0fpEmqBXx.QXzQDO0wyNwAzNzEzW}
```
First I ran /challenge/run and as instructed I pressed ctrl+C on my keyboard to give me the flag.

### New Learnings
How to interrupt a process. 

## Killing Misbehaving Processes
Sometimes, misbehaving processes can interfere with your work. These processes might need to be killed...

In this challenge, there's a decoy process that's hogging a critical resource - a named pipe (FIFO) at /tmp/flag_fifo into which (like in the Practicing Piping FIFO challenge) /challenge/run wants to write your flag. You need to kill this process.

Your general workflow should be:

1. Check what processes are running.
2. Find /challenge/decoy in the list and figure out its process ID.
3. kill it.
4. Run /challenge/run to get the flag without being overwhelmed by decoys (you don't need to redirect its output; it'll write to the FIFO on its own).
Good luck!

NOTE: You might see a few decoy flags show up even after killing the decoy process. This happens because Linux pipes are buffered: conceptually, they have a sort of length through which data flows, and you might kill the decoy process while data is in the pipe. That data, having already entered the pipe, will proceed to the other end (your cat). If you wait a second, you'll see the decoys stop, and then you can /challenge/run and win!

### Solve
**Flag: **

```bash
hacker@processes~killing-misbehaving-processes:~$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 13:54 ?        00:00:00 /sbin/docker-init -- /nix/var/nix/profiles/dojo-workspace/bin/dojo-init /run/dojo
root           7       1  0 13:54 ?        00:00:00 /run/dojo/bin/sleep 6h
root         137       1  0 13:54 ?        00:00:00 /bin/bash /challenge/.init
root         138       1  0 13:54 ?        00:00:00 /bin/bash /challenge/.init
root         139       1  0 13:54 ?        00:00:00 su -c exec /challenge/decoy > /tmp/flag_fifo hacker
root         140     138  0 13:54 ?        00:00:00 sleep 6h
root         141     137  0 13:54 ?        00:00:00 sleep 6h
hacker       142     139  0 13:54 ?        00:00:00 /usr/bin/python /challenge/decoy
hacker       144       0  0 13:54 pts/0    00:00:00 /nix/store/0nxvi9r5ymdlr2p24rjj9qzyms72zld1-bash-interactive-5.2p37/bin/bash /run
hacker       150     144  0 13:54 pts/0    00:00:00 /run/dojo/bin/bash --login
hacker       159     150  0 13:57 pts/0    00:00:00 ps -ef
hacker@processes~killing-misbehaving-processes:~$ kill 142
hacker@processes~killing-misbehaving-processes:~$ /challenge/run
Sending the flag to /tmp/flag_fifo!
hacker@processes~killing-misbehaving-processes:~$ cat /tmp/flag_fifo
pwn.college{6mHy1O85FV0Doo5G4uRVVI5CivSzV6hiWwVKpJlG4jVcua-}
pwn.college{16qp1a75BaJKqOA123ztkPRLvKvYxyXRR0BysmNvVDS8nTK}
pwn.college{tx.IbN3wejpv6xbiQKhHWmD-q2s7wkIJYp3unRBliJzMAPe}
pwn.college{Sk6Uv6tQuwByyqZWHlBtpCbM7n5yiiWmymrWW7HLD9xlNM1}
pwn.college{flOT41nRoQvlUfzCiHR-GWetz4pVC4d-m.OTiLZtLAsb-kr}
pwn.college{JgCmh7t3Vp3sNVc176b8MV53CiTXlmT8EH3W0V-JRv6E8dg}
pwn.college{tfN6tVdCncrG9.opxDEU77c-PifZWCRHb3sQfR0IIamREtu}
pwn.college{e2SPF9qyHDFsHMaIx-RbV43v6cH8pL5.Shmp8UdxGYax0fM}
pwn.college{EptlkiZ8OBa9-.T-b32aXudL20LpXUAWLnR4Gw2po8Brw01}
pwn.college{y7tXMRnS6v1mEzmehDGEtCNOvf6BX0.bF0ouNUucNAprJsm}
pwn.college{78pau.G16JCSnIWrU0cTo4i.t4-oPUxcuOnRpNEaMYtmkLu}
pwn.college{anXUoKXbngP.g9FqSgMqiooya8V-tVwkWddPbI4T5aeU40U}
pwn.college{doRv113zsDqFkNrFhauEDZaSSVo95.5eQJUf7OmBmWV6dW.}
pwn.college{W6KPgwyKA2ypfhNsHvONxkFLvR.3kgBRQRrkSd7YVxpRGcl}
pwn.college{bUkW.DcaiEqQqfZMVUaUV1NK6eIHruarVQfl6-G4OT05Os9}
pwn.college{GKmL73m9mA1hWhpqAmzvmORDjctRZcYsT4eyiBxDVx7JSVV}
pwn.college{LSVE2Dis5Obu66VRg1fAOHes596rKq5xYQMJ.CqM9CrFwgm}
pwn.college{9bKN-d9STC.RBRzH2TUSIugNASwq08hz2kqPct6zfhSCNcF}
pwn.college{qazRYn67HGgdK0Tu2fj6URflM3ohy4HMATf3HOTh-kW82fR}
pwn.college{dSBVApGKOJd9b9sP1p-6g8nvtQR93.gMhamDSStPNqA5hBO}
pwn.college{f95wZ3cuH6TtamdQsws.0zz.4-Lg52KXfGnMDLSqRaKswki}
pwn.college{cj9bwuwJFdKUde84iXKsakyRil6RnK2XXDUqGxhrkuvqWnn}
pwn.college{xycgMlAPttHqH7H9-7iSJvKr6VTQ78mujhislmDyb-vpxv6}
pwn.college{oZvG144IIdwZVNUxbgJojHn94mvxsjIuKIC.eG13UHyaA0e}
pwn.college{KT6mouBZchzkpgsiN0kEzr2UzRt5itSh37ECHnoXKvwUcxj}
pwn.college{AngzvIYmngloj4boKtTNCz.RRGPSpSikZsuKqV3Hx-jjo0Q}
pwn.college{tTcHrZXhZlvsajgvNj2ZIjgqt-JDLyNkhDT8jF.Vuu2ThQh}
pwn.college{XZmEwcC.0zu6YEG0wMzlmpADbbufw9UIejvDjsfTvsVwZRA}
pwn.college{t-5l9GTzbZALZAhb59IbT4iIwlnuyF4TB14B0zMAkVlQfag}
pwn.college{gWcVhmEFuDmRJuzGOeUokq-QGlDwOFaE4PIYjI2pYViJkVn}
pwn.college{WJGY1vL6xwiEsAIlcHA7inBRmqog2fKAlUVoOc.oee5g0eY}
pwn.college{mKAEPGpiOlNXi80C-yVTrgPJrw9stMvu3BJmQ4J5SSO0GrY}
pwn.college{kyyqicuL9jR-NN9ibM5JGF1p-bAaOUPzeoC5g4bFjzCA.bW}
pwn.college{ZepgfTVzghzBSKTq11isDvzhtHtqGlsfozoLSzD4HuV1nCc}
pwn.college{7mLdomYkfLycMpyulJ15nA.hicjgiRFarz2Ol1riFGnoewO}
pwn.college{WFZMRPns-dAHGu.nq0cs40WdLdxCB5oBj6xvWgP9VdCYLZL}
pwn.college{N-1hNzUV35CDi0ZlAyz-Fbml5wZ2DkmJKzE3Xn8qfOeYHOV}
pwn.college{2T3D4Hf26aB758nvpLsYmfHXuUVt7kR4Uj.ZDgyZ3R2Ynja}
pwn.college{l5vZ7eBmIazQDQJ2JMd5XidLzyZk6UTcRDzi.zTR10xpfzM}
pwn.college{py.gHBbBO4GKZLbJYX3Y5Zjc1J9HHDk9QXG8DTa0hkQr7qf}
pwn.college{j6m4n7puIwUq6OVyfVPpDRFr6t3U8nxB4oF9sRymeQYtrhT}
pwn.college{pjeo16200iZ2NAZZX0KWvYTxy7in-scshqLrRM-RIJUzHbz}
pwn.college{WVoc1PSRQV3Dw4kabV2wqBgVcGTBfhnFKFoyWVYaJpEwmsQ}
pwn.college{czGR52lQ431.vQ0.2XFBtbphdkOFQ2sQwSAfeEj6958R6BO}
pwn.college{YgWAB5Cbp28jIidEK8wigkwzJs7fxdpcQrpb0I4sKcAA1Cy}
pwn.college{K9YSz3OHAnkseuSk0w6qg.TsmS-hyU2Heux4d0n2W6Xj1QE}
pwn.college{M8MljB0Wz5mPLdmh1IR3YtAqw.uyLf.FxI5yumaMlbIM0iw}
pwn.college{F2DsEmDFzhwZV7qltngNm2EPp7Crl3oKqwYU6nzjYELbsHr}
pwn.college{QA.SgfK86KHmF6I6zOxHy69Ncyl1R6QOggHYS5lfnyokcJ9}
pwn.college{X-nYRJf8E13SETDin41eVgZLCVqpmJHwA6LjqAI0DqnlMrd}
pwn.college{G8aSTEkqOieiAwBEvdx9yaeoSmLCzyEIjrM1Licae4JIFWc}
pwn.college{O-9dTQ7cd3irlWzQiqf9IM-F.83ePT6DKtuiI7lDKm08nwT}
pwn.college{P9d5A.emQlMlJWAQMOscylNKYQNeowhgUYVsG9gPegO0PDB}
pwn.college{uE.NFc9h3kH-eAY.mRURzOBwhjt3M-Bu6Z6rr9i4EhUcQsT}
pwn.college{RpV4GHVwEOEsx5WyZuaAu.UCWMLSP8Tkqf7p-d13Y5hWgRC}
pwn.college{HWielFpDW.17dEFxjYv3K1pw51M6F2TKpPlFzzQmNry6t3L}
pwn.college{gxeU0smgeUDAwTai7LGSEISDBBiRA9qAgnKbFrb5-POLOvb}
pwn.college{g2dImUTm.rEtUBC0GsrGrxZSGe5B0w5geu5-YvkK7geA8LD}
pwn.college{kFCQ0QC3UjArgvpA23FIy25MQPGQy2G8LTI8d4p6q-9J8bG}
pwn.college{cmqVwYtCRBotXZ7a32ZXY14ehzLR7LqdYXVbjbGFugk52qC}
pwn.college{9U2IOfClgwRfiXTXBFwCeAbJPocm2-cGe0mMmgdY.CVoy-p}
pwn.college{0C1tqgKKHwxPVBVv7Zc0ZI38ZpgL4dP5jBj2D2mqE95oOAD}
pwn.college{Q0GKmEwvJwWKplwNyiR1KwBj6Mf1VLensmxkn08RDn5T.JA}
pwn.college{Y8E7tG2ueyHvzQsQ15tzv6K4hpSMkhJTjWDO5anW3AyafGp}
pwn.college{LHkFhXfmhlJpa66cXxAIWq1xmposVIKlpJKn9A6suyik9.S}
pwn.college{OJGN1qj0Hf7H.BF-WoVfyLUBxwlPsOrq5YJZOuEcEXnDMjn}
pwn.college{Byw0YzB6SJ0zPVqL-lmq9phxXVbYAPRwTLII1z2EFfku-T8}
pwn.college{wJKej.8uvip-k70CD53KHA4lQaFVaY76W6dOju-FYubj.XI}
pwn.college{IWvwACZINouSg51.Pi2vYddNiXwMBDUA5RdU8T4ehpO3P7k}
pwn.college{eARDTuROBfws30IzypZIEPTjTyiLIUOO-7QyME.6EkS0YLb}
pwn.college{wJKN6U-yIY6Opi0IBKKJV4IF3BJpJCNkAhJJcw.Ml0RkK.p}
pwn.college{Bq9cdHc8avubxFfsm6i-eWYSWt39r4Z96haploZRpKoJd7v}
pwn.college{e8pi9aYPkZoXpPfSLNRiVUeh6OtDPc3CJ8mfO60N344D51w}
pwn.college{LkRUAV5cCIb2a8axwB-E.j285.jX242qoT36WoLdcAuKO0S}
pwn.college{fPLbVKxwFDFIqV-gsCrnArFw.5dens0XwlFP8HFQfBLxnIK}
pwn.college{Hr4MvyWzCQeRGT8WlhdnMsskS6THf78S018v4QBGRqslspY}
pwn.college{Zssn-mklHPRs2l6C6OQ2zWVvxJWGS0jkBm0Ikcmf9gyG0yw}
pwn.college{ks06-zeUVZ0v5-t1aIxbiAXsRA7NIOvDRpz9zDf3BnLgKA5}
pwn.college{MvUsrN6tKwFQlXapzGQhWXp3.oy0aL3fE80S5OxbTeAi2OC}
pwn.college{U-2uMCquTia-X0F2.NnYAq8J4EyHTKdMRPYN9rsqel2o9wf}
pwn.college{VLUPA4ZqO5u-pdAAfM7JVSxQHa1LBYdTT.flFkNDwaPeL1t}
pwn.college{oacXvGvOdjslEaDGs2tDBcO-MsjFKXSIMhKHIYAs.5LMwHq}
pwn.college{eJbq3VCDdWptUpBcX1vsKqjdQvVIv.FP34fk5m3p0i7wo06}
pwn.college{aZ5gjycBndTFcwXPuyTXAWUZKJdhJsYKvd-Tjo00CgVCS5G}
pwn.college{cqX.hJVXhQTuQjPGYPFd-yXtshHtpphgLp7wRbi.Z6axebb}
pwn.college{P2QoAv-ix8v8OXhhexguOqXN024Q5Y7c1NUj22NJLexbsV1}
pwn.college{t-CzftPnGokqdjYAmRCOxwRsq1iclSSMGasoOrssfZnqElW}
pwn.college{6J5iFdHo3e6MgHu6nOTuDK8Iby.IlLFOh9l4HK0R4eCtPlL}
pwn.college{vxKO3B14ZnE8Y5u8xmawAmO1piNR3.2h6AZW5G4lux-IR7Z}
pwn.college{CuTKNe2PKcUP6rnMs211PzcyPB0FvDTcmALLHMH1EK6nl.G}
pwn.college{XryalfPp1Q-GyWHgxDR7d-IBn81otse5eVTffTY.u2r1qRD}
pwn.college{3oxlARP9e5nynK2pa9LeukBN576pxrsIPmWOKAgEwDDHwl6}
pwn.college{OwnRP8L-KU5uOvl7z-0wqB09D-fMiRHYWIVPZU-WqwQPL9Z}
pwn.college{XpLGAPjdTfIV18hfAmUUTRTqsr78iXiQh-W3p44BMgzP...}
pwn.college{XyGHJViGlaKQjyBRvud8cLIlVVeZfBKXHLpwhQ2i3qVDPxP}
pwn.college{3UKH7IOUCUzYDTCd69ST0nyqEfmc43pQHRZTly195LdJbL7}
pwn.college{PD5ZwnZRi-rMIGm6742w7aeh8rmlpgSgvYpg1t1hE.AH5fN}
pwn.college{pGVOBsUzfvDYvIo7rji4SESx8WipDWYLagb.ww4a3tH8vix}
pwn.college{Nobhtu.2jBFzMieuxK1WqPZaP54RPz16DVgzTIrWsde6uWE}
pwn.college{hxzAE-flBe8WR6MdK47wzAuzI0R8n-uoACnvmPL39s-9TJI}
pwn.college{RkmCNpZoSVHS1D8Vw.BS12f9v0Pex0wfGUk598NCuSO43zh}
pwn.college{uEAZgpcCyrC3S0CBxSv-ufV0uuyAaiB-.jtHK.aVDO4bt-D}
pwn.college{W8arV-rqRxE7Z0TwXeypbd7thP3XLxorl3H6HNm6MG6iny6}
pwn.college{aaJQmm7p3nYiuyEQf.6sAC3Ar6--GgJtbV98RLwUeuqwugm}
pwn.college{3TCunrR2QrnBFiI6HcJXx21JjzNC-RHrun0-j2fq0TWr38z}
pwn.college{fHOmmAWPmGB.90eQnTAkFxj9eTedgszT2mFNlRU3q.nD0aN}
pwn.college{dsXI1IGHsCM0KUPg2lJGXGqb1bTwoF3O3W9fmkV0zFgbY1c}
pwn.college{bFSKbJ0li6LP-NxtBYJDCq6x8.DR0NA5cQlyAfaCG6IVAbx}
pwn.college{mMuecwCWHaR5BW62wBR2d0eS4a7cMfNhEqaK5WE2QXocyJY}
pwn.college{iML0UMo5DuUK0G39sesmwrca3yDwvLG-jQzsscktcjXwkOY}
pwn.college{Ap8PH7nuSSMp.6rQr0srFokqA1Rg71dffQDgkVlNdTzjdJM}
pwn.college{hALIA5L.sv4gUSka5iSBfOCXNw2vjIGVpfNHs9rJVfPd.Xt}
pwn.college{kQwI21JdJ6EnRkyNICebEQ4wSE0oam7xlR-4AgimcKHFc7H}
pwn.college{UNAY16mkb8pAsR7U5yobw8It78Sf3p9JmBlmR-iylbewfNU}
pwn.college{uPt9Jwx-wIE4IvBZ40JVNsZl1fiSBPAvtWAnkoOX-DPQBCB}
pwn.college{IjoR6qG8Sh.5xCEJSLA2Jw0BKsoS8tR-XpcHzqPRvRSvOo3}
pwn.college{67Hc2g6lA6EMiYursvB-80A3dcPZklM7njSsyR7yGvshe.y}
pwn.college{3DbYWZa4yXG8teEfk3A-7FFeQzcxhRpMVrKVHDYUwZ4JzZR}
pwn.college{ml6DYJXGMHhe9PjRF-BhNUKESXjT5p8ATjVHRl.YFTEICup}
pwn.college{xSWDA-n7VdL3tcYnluph1gffODfY68PhfVyjQQ.POaTwO-N}
pwn.college{IId85p3XrmhNdrOiUAdzam5DGySDEVJMiU2rjaAUYkglyG3}
pwn.college{XQzNZz93EPmISKYyeqTzxoZt5bVEnzF2.Y.EoK08GxZYYA.}
pwn.college{9dhD9sCs9K37VzBGoK50cv0ljuD7OlHzeYBm9Y1HW5TggZy}
pwn.college{WXCAsBFi-P0dUL3AePuvyPGKF4Oe5vJdGgvZk5I-Lv4GwYp}
pwn.college{NQFk7EqUzJuTTwWLmH.ikgBpOgNeaJ0KlSN8iJYWB.-8WP1}
pwn.college{qjoTHGERyr2P-KgM7Q188gD1uqawYs4ZMDconPc6f6Rfn3r}
pwn.college{zbCYA0sW5DJQIgY7mhci65Qz5nZuG9x5Lql5l-RMU9m5PI3}
pwn.college{QubdS9-VhATLz4wZ2FpfWSZCKQA0JgRVqk1hAgVclerX5Xo}
pwn.college{rAWjTNVv.6Mugw-7Zwsa5bZ7AHk1SxYzvk9.L1DVi1vsOqX}
pwn.college{Mv7OeNpQMPmVXDB24cQUOO7pN1ciUehiTfQnbB8jbH7ReM3}
pwn.college{WPztDTp.3IW8uiDCC74uLfFWL.2.Vg6uOz1MBZSZf4PHFpF}
pwn.college{R5AYxpe.fWYeBicFumGO4Rzmb0Ya5C7G8Jx2la1x6.9pj64}
pwn.college{UYoaEDYDmtSFEH-kt7Bd7-585d1w2Bcoy2sJt38TZLX279.}
pwn.college{hcYQrRFALHTehlqeVk28Q0.dwhZqkUmu2LlkueX9u4vN6iG}
pwn.college{YREag2f-JhhxHGMF1oR5ADcYN93.0FNzMDOxwyNwAzNzEzW}
```
I ran ps -ef command to display the list of ongoing processes to obtain the PID of /challenge/decoy, when I got the PID I used the kill command to terminate it and then ran /challenge/run to send the flag to /tmp/flag_fifo, exiting the shell and reloading it and catted the content of /tmp/flag_fifo displaying a lot of decoy flags but the last flag is the real flag as mentioned in NOTE.

### New Learnings
Better understanding of how to use kill command to terminate a process.

## Suspending Processes
You have learned to interrupt processes with Ctrl-C, but there are less drastic measures you can use to get your terminal back! You can suspend processes to the background with Ctrl-Z. In this level, we'll explore how this works and, in the next level, we'll figure out how to resume those suspended processes!

This level's run wants to see another copy of itself running and using the same terminal. How? Use the terminal to launch it, then suspend it, then launch another copy while the first is suspended!

### Solve
**Flag: pwn.college{8-rMp8Lc_noPSjIOzJDrhlYsulC.QX1QDO0wyNwAzNzEzW}**

```bash
hacker@processes~suspending-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running in
this terminal... Let's check!

UID          PID    PPID  C STIME TTY          TIME CMD
root         139     129  0 14:31 pts/0    00:00:00 bash /challenge/run
root         141     139  0 14:31 pts/0    00:00:00 ps -f

I don't see a second me!

To pass this level, you need to suspend me and launch me again! You can
background me with Ctrl-Z or, if you're not ready to do that for whatever
reason, just hit Enter and I'll exit!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~suspending-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running in
this terminal... Let's check!

UID          PID    PPID  C STIME TTY          TIME CMD
root         139     129  0 14:31 pts/0    00:00:00 bash /challenge/run
root         146     129  0 14:32 pts/0    00:00:00 bash /challenge/run
root         148     146  0 14:32 pts/0    00:00:00 ps -f

Yay, I found another version of me! Here is the flag:
pwn.college{8-rMp8Lc_noPSjIOzJDrhlYsulC.QX1QDO0wyNwAzNzEzW}
```
First I ran /challenge/run, then suspended it by doing ctrl + z on the keyboard and then ran it again to get the flag

### New Learnings
How to suspend a process temporarily to the background using ctrl + z on the keyboard.

## Resuming Processes
Usually, when you suspend processes, you'll want to resume them at some point. Otherwise, why not just terminate them? To resume processes, your shell provides the fg command, a builtin that takes the suspended process, resumes it, and puts it back in the foreground of your terminal.

Go try it out! This challenge's run needs you to suspend it, then resume it. Good luck!

### Solve
**Flag: pwn.college{8wCPYLhElhHs4votDbYcblNhwry.QX2QDO0wyNwAzNzEzW}**

```bash
hacker@processes~resuming-processes:~$ /challenge/run
Let's practice resuming processes! Suspend me with Ctrl-Z, then resume me with
the 'fg' command! Or just press Enter to quit me!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~resuming-processes:~$ fg
/challenge/run
I'm back! Here's your flag:
pwn.college{8wCPYLhElhHs4votDbYcblNhwry.QX2QDO0wyNwAzNzEzW}
Don't forget to press Enter to quit me!

Goodbye!
```
First I ran /challenge/run then suspended it by using ctrl + z and then ran the fg command to resume it and obtained the flag

### New Learnings
How to resume a process after suspending it.

## Backgrounding Processes 
You've resumed processes in the foreground with the fg command. You can also resume processes in the background with the bg command! This will allow the process to keep running, while giving you your shell back to invoke more commands in the meantime.

This level's run wants to see another copy of itself running, not suspended, and using the same terminal. How? Use the terminal to launch it, then suspend it, then background it with bg and launch another copy while the first is running in the background!

ARCANUM: If you're interested in some deeper details, check out how to view the differences between suspended and backgrounded properties! Allow me to demonstrate. First, let's suspend a sleep:
```bash
hacker@dojo:~$ sleep 1337
^Z
[1]+  Stopped                 sleep 1337
hacker@dojo:~$
```
The sleep process is now suspended in the background. We can see this with ps by enabling the stat column output with the -o option:
```bash
hacker@dojo:~$ ps -o user,pid,stat,cmd
USER         PID STAT CMD
hacker       702 Ss   bash
hacker       762 T    sleep 1337
hacker       782 R+   ps -o user,pid,stat,cmd
hacker@dojo:~$ 
```
See that T? That means that the process is suspended due to our Ctrl-Z. The S in bash's STAT column means that bash is sleeping while waiting for input. The R in ps's column means that it's actively running, and the + means that it's in the foreground!

Watch what happens when we resume sleep in the background:
```bash
hacker@dojo:~$ bg
[1]+ sleep 1337 &
hacker@dojo:~$ ps -o user,pid,stat,cmd
USER         PID STAT CMD
hacker       702 Ss   bash
hacker       762 S    sleep 1337
hacker      1224 R+   ps -o user,pid,stat,cmd
hacker@dojo:~$
```
Boom! The sleep now has an S. It's sleeping while, well, sleeping, but it's not suspended! It's also in the background and thus doesn't have the +.

### Solve
**Flag: pwn.college{gxMge5Uln42p5lLC13M9u4suENt.QX3QDO0wyNwAzNzEzW}**

```bash
hacker@processes~backgrounding-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running *and
not suspended* in this terminal... Let's check!

UID          PID STAT CMD
root         138 S+   bash /challenge/run
root         140 R+   ps -o user=UID,pid,stat,cmd

I don't see a second me!

To pass this level, you need to suspend me, resume the suspended process in the
background, and then launch a new version of me! You can background me with
Ctrl-Z (and resume me in the background with 'bg') or, if you're not ready to
do that for whatever reason, just hit Enter and I'll exit!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~backgrounding-processes:~$ bg
[1]+ /challenge/run &
hacker@processes~backgrounding-processes:~$


Yay, I'm now running the background! Because of that, this text will probably
overlap weirdly with the shell prompt. Don't panic; just hit Enter a few times
to scroll this text out.

hacker@processes~backgrounding-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running *and
not suspended* in this terminal... Let's check!

UID          PID STAT CMD
root         138 S    bash /challenge/run
root         148 S    sleep 6h
root         151 S+   bash /challenge/run
root         153 R+   ps -o user=UID,pid,stat,cmd

Yay, I found another version of me running in the background! Here is the flag:
pwn.college{gxMge5Uln42p5lLC13M9u4suENt.QX3QDO0wyNwAzNzEzW}
```
I ran /challenge/run and then suspended it by ctrl + z, then I used the bg command to background the process and then ran it again

### New Learnings
How to resume a process in the background  

## Foregrounding processes 
Imagine that you have a backgrounded process, and you want to mess with it some more. What do you do? Well, you can foreground a backgrounded process with fg just like you foreground a suspended process! This level will walk you through that!

### Solve
**Flag: pwn.college{0KOyH_KAMu9DAWKHKy1zFoBVwQM.QX4QDO0wyNwAzNzEzW}**

```bash
hacker@processes~foregrounding-processes:~$ /challenge/run
To pass this level, you need to suspend me, resume the suspended process in the
background, and *then* foreground it without re-suspending it! You can
background me with Ctrl-Z (and resume me in the background with 'bg') or, if
you're not ready to do that for whatever reason, just hit Enter and I'll exit!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~foregrounding-processes:~$ bg
[1]+ /challenge/run &
hacker@processes~foregrounding-processes:~$


Yay, I'm now running the background! Because of that, this text will probably
overlap weirdly with the shell prompt. Don't panic; just hit Enter a few times
to scroll this text out. After that, resume me into the foreground with 'fg';
I'll wait.

hacker@processes~foregrounding-processes:~$ fg
/challenge/run
YES! Great job! I'm now running in the foreground. Hit Enter for your flag!

pwn.college{0KOyH_KAMu9DAWKHKy1zFoBVwQM.QX4QDO0wyNwAzNzEzW}
```
First I ran /challenge/run and then suspended it by ctrl + z, then I used the bg command to background and then used the fg command to run it into foreground and obtained the flag. 

### New Learnings
More depth learning into backgrounding and foregrounding a process.

## Starting Background Processes
Of course, you don't have to suspend processes to background them: you can start them backgrounded right off the bat! It's easy; all you have to do is append a & to the command, like so:
```bash
hacker@dojo:~$ sleep 1337 &
[1] 1771
hacker@dojo:~$ ps -o user,pid,stat,cmd
USER         PID STAT CMD
hacker      1709 Ss   bash
hacker      1771 S    sleep 1337
hacker      1782 R+   ps -o user,pid,stat,cmd
hacker@dojo:~$ 
```
Here, sleep is actively running in the background, not suspended. Now it's your turn to practice! Launch /challenge/run backgrounded for the flag!

### Solve
**Flag: pwn.college{ctPmkSC1DLaDsVbRKBr5R3m_1vs.QX5QDO0wyNwAzNzEzW}**

```bash
hacker@processes~starting-backgrounded-processes:~$ /challenge/run &
[2] 139
[1]   Exit 127                /challenge/flag
hacker@processes~starting-backgrounded-processes:~$


Yay, you started me in the background! Because of that, this text will probably
overlap weirdly with the shell prompt, but you're used to that by now...

Anyways! Here is your flag!
pwn.college{ctPmkSC1DLaDsVbRKBr5R3m_1vs.QX5QDO0wyNwAzNzEzW}
```
I ran /challenge/run in the background with & as the argument

### New Learnings


## Process Exit Codes 
Every shell command, including every program and every builtin, exits with an exit code when it finishes running and terminates. This can be used by the shell, or the user of the shell (that's you!) to check if the process succeeded in its functionality (this determination, of course, depends on what the process is supposed to do in the first place).

You can access the exit code of the most recently-terminated command using the special ? variable (don't forget to prepend it with $ to read its value!):
```bash
hacker@dojo:~$ touch test-file
hacker@dojo:~$ echo $?
0
hacker@dojo:~$ touch /test-file
touch: cannot touch '/test-file': Permission denied
hacker@dojo:~$ echo $?
1
hacker@dojo:~$
```
As you can see, commands that succeed typically return 0 and commands that fail typically return a non-zero value, most commonly 1 but sometimes an error code that identifies a specific failure mode.

In this challenge, you must retrieve the exit code returned by /challenge/get-code and then run /challenge/submit-code with that error code as an argument. Good luck!

### Solve
**Flag: pwn.college{s0hLKhu1crSIrlcp-1dsK_sYYJc.QX5YDO1wyNwAzNzEzW}**

```bash
hacker@processes~process-exit-codes:~$ /challenge/get-code
Exiting with an error code!
hacker@processes~process-exit-codes:~$ echo $?
183
hacker@processes~process-exit-codes:~$ /challenge/submit-code 183
CORRECT! Here is your flag:
pwn.college{s0hLKhu1crSIrlcp-1dsK_sYYJc.QX5YDO1wyNwAzNzEzW}
```
I ran ?challenge/get-code and then echoed the error code so that it can print the secret code as the error, then I ran /challenge/submit-code with 183 as the argument input to get the flag

### New Learnings
How to run Process exit codes and what are they.


