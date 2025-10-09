# Terminal Multiplexing 

## Multiplexing Launching Screen
Let's dive right in!

screen is a program that creates virtual terminals inside your terminal. It's somewhat like having multiple browser tabs, but for your command line!

Starting screen is super simple:
```bash
hacker@dojo:~$ screen
```
That's it! You're now inside a screen session. It looks exactly like a terminal, but there are new capabilities there, waiting to be discovered.

For this challenge, we've hooked things up so that just launching screen will get you the flag. Easy!

NOTE: When you're done with your command line, type exit or press Ctrl-D to leave the screen session. Then screen will terminate and return you to your original shell.

### Solve
**Flag: pwn.college{4K2SoE8mf1GDv-muBLGJ_2sEEPm.0VN4IDOxwyNwAzNzEzW}**

```bash
hacker@terminal-multiplexing~launching-screen:~$ screen
Congratulations! You're inside a screen session!
Here's your flag:
pwn.college{4K2SoE8mf1GDv-muBLGJ_2sEEPm.0VN4IDOxwyNwAzNzEzW}
hacker@terminal-multiplexing~launching-screen:~$ exit
[screen is terminating]
hacker@terminal-multiplexing~launching-screen:~$
```
I ran the screen command to open another window of shell and then, get the flag, and exit the screen to come back to the original screen.

### New Learnings
How to use the screen command to open another window of shell and exit command to go back to the original screen.

## Detatching and Attaching 
Now we'll start digging in with the magic of detaching!

Imagine you're working on something important over a remote connection, and your connection drops. With a normal terminal (outside of this awesome dojo environment), everything's gone. With screen, your work keeps running, and you can reattach later!

You can also detach on purpose, which we'll do in this challenge. You detach by pressing Ctrl-A, followed by d (for detach). This leaves your session running in the background while you return to your normal terminal.
```bash
hacker@dojo:~$ screen
[doing some work...]
[Press Ctrl-A, then d]
[detached from 12345.pts-0.hostname]
hacker@dojo:~$ 
```
To reattach, you can use the -r argument to screen:
```bash
hacker@dojo:~$ screen -r
```
For this challenge, you'll need to:

1. Launch screen
2. Detach from it.
3. Run /challenge/run (this will secretly send the flag to your detached session!)
4. Reattach to see your prize
FUN FACT: Ctrl-A is screen's activation key for all of its shortcuts in its default configuration. All screen functionality is activated by some command combination starting with Ctrl-A.

HINT: Remember: Hold Ctrl and press A, then release both and press d.

HINT: If you see [detached from...], you did it right!

### Solve
**Flag: pwn.college{soXtycnF0v1EDHub7GdgQalk3qx.0lN4IDOxwyNwAzNzEzW}**

```bash
hacker@terminal-multiplexing~detaching-and-attaching:~$ screen
[detached from 154.pts-1.terminal-multiplexing~detaching-and-attaching]
hacker@terminal-multiplexing~detaching-and-attaching:~$ /challenge/run
Found detached screen session: 154.pts-1.terminal-multiplexing~detaching-and-attaching
Sending flag to your screen session...

Flag sent! Now reattach to your screen session with:

  screen -r

You'll find the flag waiting for you there!
hacker@terminal-multiplexing~detaching-and-attaching:~$ screen -r
hacker@terminal-multiplexing~detaching-and-attaching:~$ echo Yes! Flag is: pwn.college{soXtycnF0v1EDHub7GdgQalk3qx.0lN4IDOxwyNwAzNzEzW}
```
I ran the screen command to enter another window, and then pressed ctrl+A and then d to detatch from that window and keep it running in the background, then I ran /challenge/run and snet the flag to the window running in the background and then reattached the screen to obtain the flag.

### New Learning
How to detatch and attach screen windows.

## Finding sessions 
Time for some screen detective work!

If you become an avid screen user, you will inevitably end up with multiple sessions running. How do you find the right one to reattach to?

Well, we can list them:
```bash
hacker@dojo:~$ screen -ls
There are screens on:
        23847.mysession   (Detached)
        23851.goodwork    (Detached)
        23855.morework    (Detached)
3 Sockets in /run/screen/S-hacker.
```
The identifiers of the sessions are the PID of each respective screen process, a dot, and the name of the screen session. To attach to a specific one, you use its name or its PID by giving it as an argument to screen -r.
```bash
hacker@dojo:~$ screen -r goodwork
```
In this challenge, we've created three screen sessions for you. One of them contains the flag. The other two are decoys!

You'll need to check each one until you find it. Don't forget to detach (Ctrl-A d) before trying the next session!

### Solve
**Flag: pwn.college{IycQwM5VWaOAtmPvWkIod-zeH1e.01N4IDOxwyNwAzNzEzW}**

```bash
hacker@terminal-multiplexing~finding-sessions:~$ screen -ls
There are screens on:
        135.challenge_session   (Remote or dead)
        144.session_a392f8cf9e134bc2    (Detached)
        147.session_c3f8fd1bf135824c    (Detached)
        150.session_0c58cb749f1d864e    (Detached)
4 Sockets in /home/hacker/.screen.
hacker@terminal-multiplexing~finding-sessions:~$ screen -r 144
hacker@terminal-multiplexing~finding-sessions:~$  echo 'Nothing here! Try another session.'
Nothing here! Try another session.
hacker@terminal-multiplexing~finding-sessions:~$ exit
hacker@terminal-multiplexing~finding-sessions:~$ screen -r 147
hacker@terminal-multiplexing~finding-sessions:~$  echo 'Congratulations! You found the right session!'
Congratulations! You found the right session!
hacker@terminal-multiplexing~finding-sessions:~$  echo pwn.college{IycQwM5VWaOAtmPvWkIod-zeH1e.01N4IDOxwyNwAzNzEzW}
pwn.college{IycQwM5VWaOAtmPvWkIod-zeH1e.01N4IDOxwyNwAzNzEzW}
```
I checked the screen listings and ran the screen sessions one by one until the flag printed.

### New Learning
How to list the screens running in the background, and how to reattach screens by choice.


## Switching windows 
Okay, so far, screen is just a weird sort of terminal-with-a-terminal. But it can be much more!

Inside a single screen session, you can have multiple windows, like your browser has multiple tabs. This can be super handy for organizing different tasks!

These windows are handled with different keyboard shortcuts, all starting with Ctrl-A:

- Ctrl-A c - Create a new window
- Ctrl-A n - Next window
- Ctrl-A p - Previous window
- Ctrl-A 0 through Ctrl-A 9 - Jump directly to window 0-9
- Ctrl-A " - bring up a selection menu of all of the windows
For this challenge, we've set up a screen session with two windows:

- Window 0 has... well, you'll have to switch there to find out!
- Window 1 has a welcome message
Attach to the session with screen -r, then use one of the key combinations above to switch to Window 1. Go get that flag!

### Solve
**Flag: pwn.college{QzmElJnH1x_nU1UJDMUwdP5u5Y4.0FO4IDOxwyNwAzNzEzW}**

```bash
hacker@terminal-multiplexing~switching-windows:~$ screen -ls
There are screens on:
        135.challenge_session   (Remote or dead)
        147.session_c3f8fd1bf135824c    (Remote or dead)
        150.session_0c58cb749f1d864e    (Remote or dead)
        134.challenge_session   (Detached)
4 Sockets in /home/hacker/.screen.
hacker@terminal-multiplexing~switching-windows:~$ screen -r 134
 cat <<MSG
Welcome to the window switching challenge!
You are currently in window 1.
The flag is hidden in window 0.
Use Ctrl-A 0 to switch to window 0!
MSG
hacker@terminal-multiplexing~switching-windows:~$  cat <<MSG
> Welcome to the window switching challenge!
> You are currently in window 1.
> The flag is hidden in window 0.
> Use Ctrl-A 0 to switch to window 0!
> MSG
Welcome to the window switching challenge!
You are currently in window 1.
The flag is hidden in window 0.
Use Ctrl-A 0 to switch to window 0!
hacker@terminal-multiplexing~switching-windows:~$  cat <<MSG
> Excellent work! You found window 0!
> Here is your flag: pwn.college{QzmElJnH1x_nU1UJDMUwdP5u5Y4.0FO4IDOxwyNwAzNzEzW}
> MSG
Excellent work! You found window 0!
Here is your flag: pwn.college{QzmElJnH1x_nU1UJDMUwdP5u5Y4.0FO4IDOxwyNwAzNzEzW}
```
I first checked the listing to find the challenge session, I reattached the window by running its PID with -r and figured out to switch window to 0 to get the flag which I did by pressing ctrl+A and then 0 and obtained the flag. 

### New Learning
How to switch between windows.

## Detatching and Attaching(tmux)
Let's try the same thing with tmux!

tmux (terminal multiplexer) is screen's younger, more modern cousin. It does all the same things but with some different key bindings. The biggest difference? Instead of Ctrl-A, tmux uses Ctrl-B as its command prefix.

So to detach from tmux, you press Ctrl-B followed by d.
```bash
hacker@dojo:~$ tmux
[doing some work...]
[Press Ctrl-B, then d]
[detached (from session 0)]
hacker@dojo:~$ 
```
The commands also differ:

- tmux ls - List sessions
- tmux attach or tmux a - Reattach to session
For this challenge:

1. Launch tmux
2. Detach from it.
3. Run /challenge/run (this will send the flag to your detached session!)
4. Reattach to see your prize

### Solve
**Flag: **

```bash
hacker@terminal-multiplexing~detaching-and-attaching-tmux:~$ tmux
[detached (from session 0)]
hacker@terminal-multiplexing~detaching-and-attaching-tmux:~$ /challenge/run
Found detached tmux session: 0
Sending flag to your tmux session...

Flag sent! Now reattach to your tmux session with:
  tmux attach

You'll find the flag waiting for you there!
hacker@terminal-multiplexing~detaching-and-attaching-tmux:~$ tmux a
hacker@terminal-multiplexing~detaching-and-attaching-tmux:~$  echo Congratulations, here is your flag: pwn.college{0KyVBFDlYqyjKvwjRF262-Q6Jaq.0VO4IDOxwyNwAzNzEzW}
Congratulations, here is your flag: pwn.college{0KyVBFDlYqyjKvwjRF262-Q6Jaq.0VO4IDOxwyNwAzNzEzW}
```
I first ran tmux command to go into another window and then detatched from that window, then I ran /challenge/run to send flag into the window running in the background, then I reattached the window by using tmux with the argument of a to obtain the flag. 

### New Learning
How to use the tmux command its similarities and differences with screen command.

## Switching Windows(tmux)
Let's learn to navigate windows in tmux!

Just like screen, tmux has windows. The key combos are different, but the concept is the same:

- Ctrl-B c - Create a new window
- Ctrl-B n - Next window
- Ctrl-B p - Previous window
- Ctrl-B 0 through Ctrl-B 9 - Jump to window 0-9
- Ctrl-B w - See a nice window picker
Tmux shows your windows at the bottom in a status bar that looks like:
```bash
[0] 0:bash* 1:bash
```

The * shows your current window, and each entry also shows the process that the window was created to run.

We've created a tmux session with two windows:

- Window 0 has the flag!
- Window 1 has your warm welcome.
Go get that flag!

### Solve
**Flag: pwn.college{EGfcBnGLKKQkam0yrSc9PYmsaJv.0FM5IDOxwyNwAzNzEzW}**

```bash
hacker@terminal-multiplexing~switching-windows-tmux:~$ tmux ls
challenge_session: 2 windows (created Thu Oct  9 16:06:25 2025)
hacker@terminal-multiplexing~switching-windows-tmux:~$ tmux a
 cat <<MSG
Welcome to the tmux window switching challenge!
You are currently in window 1.
The flag is hidden in window 0.
Use Ctrl-B 0 to switch to window 0!
MSG
hacker@terminal-multiplexing~switching-windows-tmux:~$  cat <<MSG
> Welcome to the tmux window switching challenge!
> You are currently in window 1.
> The flag is hidden in window 0.
> Use Ctrl-B 0 to switch to window 0!
> MSG
Welcome to the tmux window switching challenge!
You are currently in window 1.
The flag is hidden in window 0.
Use Ctrl-B 0 to switch to window 0!
hacker@terminal-multiplexing~switching-windows-tmux:~$  cat <<MSG
> Excellent work! You found window 0!
> Here is your flag: pwn.college{EGfcBnGLKKQkam0yrSc9PYmsaJv.0FM5IDOxwyNwAzNzEzW}
> MSG
Excellent work! You found window 0!
Here is your flag: pwn.college{EGfcBnGLKKQkam0yrSc9PYmsaJv.0FM5IDOxwyNwAzNzEzW}
hacker@terminal-multiplexing~switching-windows-tmux:~$
```
I first checked the listing to find the challenge session, I reattached the window by running tmux with argument a and figured out to switch window to 0 to get the flag which I did by pressing ctrl+B and then 0 and obtained the flag. 

### New Learning
How to switch between windows using the tmux command.

