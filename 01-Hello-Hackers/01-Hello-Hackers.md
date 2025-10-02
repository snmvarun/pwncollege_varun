# Module: Hello Hackers

## Intro to Commands
In this challenge, you will invoke your first command! When you type a command and hit enter, the command will be invoked, as so:
```bash
hacker@dojo:~$ whoami
hacker
hacker@dojo:~$
```
Here, the user executed the whoami command, which simply prints the username (hacker) to the terminal. When the command terminates, the shell once again displays the prompt, ready for the next command.

In this level, invoke the hello command to get the flag! Keep in mind: commands in Linux are case sensitive: hello is different from HELLO.

### Solve
**Flag: pwn.college{cUZTZtBbfyPd-W-Ve2fZDKR90MB.QX3YjM1wyNwAzNzEzW}**

```bash
hacker@hello~intro-to-commands:~$ whoami
hacker
hacker@hello~intro-to-commands:~$ hello
Success! Here is your flag:
pwn.college{cUZTZtBbfyPd-W-Ve2fZDKR90MB.QX3YjM1wyNwAzNzEzW}
```

as instructed, I executed the whoami command and it prints a statement "hacker", writing hello gives me the flag to this challenge, indicating success.

### New Learnings
How to connect using ssh to wsl, and access my public key. And also using commands like whoami and hello.

## Intro to Arguements
Let's try something more complicated: a command with arguments, which is what we call additional data passed to the command. When you type a line of text and hit enter, the shell actually parses your input into a command and its arguments. The first word is the command, and the subsequent words are arguments. Observe:
```bash
hacker@dojo:~$ echo Hello
Hello
hacker@dojo:~$
```
In this case, the command was echo, and the argument was Hello. echo is a simple command that "echoes" all of its arguments back out onto the terminal, like you see in the session above.

Let's look at echo with multiple arguments:
```bash
hacker@dojo:~$ echo Hello Hackers!
Hello Hackers!
hacker@dojo:~$
```
In this case, the command was echo, and Hello and Hackers! were the two arguments to echo. Simple!

In this challenge, to get the flag, you must run the hello command (NOT the echo command) with a single argument of hackers. Try it now!

### Solve
**Flag: pwn.college{Q8LSt6uZKKxdEmV0XbeL3ghHrdI.QX4YjM1wyNwAzNzEzW}**
```bash
hacker@hello~intro-to-arguments:~$ echo Hello
Hello
hacker@hello~intro-to-arguments:~$ echo Hello Hackers!
Hello Hackers!
hacker@hello~intro-to-arguments:~$ hello hackers
Success! Here is your flag:
pwn.college{Q8LSt6uZKKxdEmV0XbeL3ghHrdI.QX4YjM1wyNwAzNzEzW}
```

echo command prints the statements given by the user, in this case "echo" was the command and "Hello" and "Hackers!" were the arguements.
to obtain the flag I needed to run the hello as a command and hackers as an arguement, therefore making "hello hackers", successfully giving me the flag.

### New Learnings
I learnt what were the commands and the arguements, and the echo command.

## Command History
You're going to type a lot of commands, and typing everything from scratch can be annoying. Luckily, the shell saves a history of every command you invoke.

You can scroll through those saved commands with the up/down arrow keys, and we'll practice that in this challenge. This challenge will inject the flag into your history. Bring up a terminal, hit the up arrow, and grab it! In other challenges, the history will contain the log of the commands you've run, so if you need to run a similar command again, you can use the arrow keys to scroll through and find it!

### Solve 
**Flag: pwn.college{Q8LSt6uZKKxdEmV0XbeL3ghHrdI.QX4YjM1wyNwAzNzEzW}**

```bash
hacker@hello~command-history:~$ the flag is pwn.college{smvVzdGjW-NNExuiyJ42rrhVUpt.0lNzEzNxwyNwAzNzEzW}
```

I pressed the up arrow key to check my history of lines I entered and the statement above was found, I extracted the flag and successfully finished the challenge

### New Learnings
Learned how to browse through the history of previous lines of code.
