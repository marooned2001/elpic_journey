# **105.1 Costumize and use the shell environment**
## **Environment variable**
Shell enviornment variables are cotain information and configuration about shell.
## **env**
In Linux, the environment is a set of key=value pairs that the shell passes to every process it starts. Programs read these variables to know things like where to find executables, what language/locale to use, who the current user is, etc.
### Common variables worth knowing
Variable|Purpose
--|--
PATH|Colon-separated list of directories searched for executables
HOME|Current user's home directory
USER|Current username
SHELL|Path to the user's default shell
PWD|Current working directory
PS1|Primary shell prompt string

## **set**
`set` allows you to change the values of shell options (variables) or to display the names and values of shell variables.
Using `set` we can configure how bash works. These are some samples:
Using set we can configure how bash works.
### Common switches
Option | long form| Effect
-|-|-
-e|errexit|Exit immediately if a command fails (non-zero status)
-x|xtrace|Print each command before executing it (great for debugging scripts)
-u|nounset|Error out if you reference an unset variable
-v|verbose|Print shell input lines as read
-n|noexec|Read commands but don't execute (syntax check)
-f|noglob|isable filename expansion (globbing, e.g. *.txt)
-o|(named option)|Set an option by name, e.g. set -o pipefail
-c|noclobber|Prevent > from overwriting existing files

## Setting environment variables
Bash environment variables can contain only letters (a to z or A to Z), numbers ( 0 to 9) or the underscore character and cannot start with numbers. Traditionally we use UPPERCASE letters in our variable names.An environment variable is just a name=value pair that gets passed down to any program the shell launches. Here's how to actually set one.
```
Note:There should be no spaces around =.
Use the quotes or double-quotes when your value do have spaces (or other special characters) in it.
Use a dollar sign only when referring to a variable, and not when defining it.
```

## unset
This command unsets a variable or a function.

## export
`export` is a shell builtin that marks a shell variable so it gets passed down into the environment of any child process the shell starts.
>Note:`export` only affects processes started after the `export` happens. Variables already running elsewhere (an already-open terminal, an already-running background process) never retroactively see it. And like everything set at the prompt, it vanishes when the shell closes — to persist it, put the export line in `~/.bashrc` or `~/.profile` and reload with `source`.

## .(and source)
This is a shortcut for the bash source command. You can find it in files like `/etc/profile`. It runs the executable in front of it as part of the current environment (and not in a sub-process).

## Aliases
An alias is a custom name that expands to a longer command (or command with options) before the shell runs it.
### example
```bash
alias ll='ls -la'
```
Now typing `ll` runs `ls -la`.

>Note: `alias` command (No argumans) is for listing aliases.

## functions
A function groups a sequence of commands under a name, and — unlike aliases — it can accept arguments, use logic, and return values. This is the tool to reach for whenever an alias isn't enough.
### basic syntax
```bash
function myfunc {
    echo "hello"
}
```

## **Different shell envs**
The shell can be started in 3 different ways:
1. login shell
2. interactive shell
3. non interactive shell

## login shell
Think of it like this: the first shell you get when you log in is the "login shell." Any shell you open after that (like a new terminal tab) is just a regular shell.
After user passed the usename and pasword and click on `enter` this is step by step: 
1. It reads the system-wide `/etc/profile` (and usually `/etc/profile.d/*.sh` from there).
2. The shell checks three possible personal files in this exact order, and stops as soon as it finds one that exists:
    1. `~/.bash_profile` — checked first
    2. `~/.bash_login` — only checked if #1 doesn't exist
    3. `~/.profile` — only checked if neither #1 nor #2 exist

    >Note: it does not read all three, even if more than one exists. It picks the first match and ignores the rest.
3. At the end, the system loads: `/home/USERNAME/.bashrc`


## Interactive (non-login) shell
This is what you get most of the time on a desktop — e.g. opening a newterminal window/tab, or typing bash inside another shell. You're alreadylogged in; this is just a fresh shell session for typing commandsinteractively.
Two file will handle the environment:

1. /etc/bash.bashrc (or /etc/bashrc in some systems)
2. /home/USERNAME/.bashrc


## Non Interactive Shell
This is a shell running without a human typing commands — most commonly when you execute a script.
There is no specific file to be used in this case. Instead we have the BASH_ENV. When a new non-interactive shell starts, it looks into this variable and if it points to a file, runs it.

## **A few more files**

## /etc/skel
``/etc/skel`` (short for "skeleton") is a template directory. Whatever files sit inside it get automatically copied into a new user's home directory the moment that user account is created

## .bash_logout
This file runs cleanup commands automatically when a login shell exits — the opposite bookend of `.bash_profile`. Where `.bash_profile` runs at the start of a login session, `~/.bash_logout` runs at the end of it.

# **105.2 Customize or write simple script**
## **Combining Commands**
Some time you need to run more than one commands in one line, In this case we separate them using `;`.
### example
```bash
cd /home ; ls  #it will run `cd /home` after that `ls`
```

## **Shell scripts**
A shell script is just a text file full of the same commands you'd type one by one at the terminal — saved so you can run them all at once, repeatedly, without retyping.We can combine shell commands or programs and or some logic or loops to write large or small scripts. Shell scripts are useful when you want to automate some tasks in one larger command.

### **Shebang**
The shebang (`#!`) is the first line of a script — it tells the system which interpreter should execute the file.
Without it, the system has to guess how to run your script, or you have to specify the interpreter manually every time (`bash script.sh`). With it, `./script.sh` just works, and the script is self-contained about what it needs to run.

### **Variables**
A variable is a named container for a value that you can reuse and update throughout a script instead of hardcoding it everywhere.Change the value once, and everything that depends on it updates automatically. It's the difference between a script you can maintain and one you have to rewrite every time something changes.

#### example
```bash
name="Mohamad"
echo "Hello, $name" #will show "Hello mohamed"
```

### **Command substitution**
Command substitution lets you take the output of one command and use it as a value  (inside a variable, another command, or a string) instead of typing that output by hand.
#### example
```bash
today=$(date +%F)
echo "Today is $today"
```
`$(...)` is the modern, preferred syntax — it's nestable and easier to read.
Backticks `` `comand` `` do the same thing but are the older syntax and harder to nest cleanly.
The shell runs the command first, then substitutes its stdout output right into place.

### **executing scripts**
There are a few ways to run a script, and the difference between them actually matters — not just syntax preference.Some methods run the script in a new subprocess; others run it in your current shell. That distinction decides whether things like variable changes or `cd` commands inside the script affect your terminal session afterward.
#### example
```bash
./script.sh        # needs execute permission (chmod +x); runs in a new subshell
bash script.sh      # doesn't need execute permission; runs in a new subshell
sh script.sh        # runs with the sh interpreter, in a new subshell
. script.sh         # "dot" — runs in the CURRENT shell
source script.sh    # same as above; runs in the CURRENT shell
```
* `./script.sh` and bash `script.sh` spawn a child process — any ``cd`` or exported variables inside the script disappear once it finishes.
* `. script.sh `/ `source script.sh` runs the script's commands as if you typed them yourself — useful for things like loading environment variables into your current session (e.g. `. ~/.bashrc`).
* `./script.sh` requires both execute permission and the shebang line to work correctly.

### **Conditions**
Conditions let a script make decisions — run different commands depending on whether something is true, a file exists, or a command succeeded.
Without conditions, a script just runs top to bottom no matter what. Conditions are what make a script actually smart — able to handle different situations instead of blindly executing the same steps every time.
#### example
```bash
if [ -f "/etc/passwd" ]; then
    echo "File exists"
else
    echo "File not found"
fi
```
* `[ ]` is actually a command (`test`) — the spaces around the brackets are required, not stylistic.
* Common test flags: `-f` (file exists), `-d` (directory exists), `-z` (string is empty), ``-eq``/`-ne` (numeric comparison), `=`/`!=` (string comparison).
* In shell scripting, `0` means success/true and anything else means failure/false — the opposite of what you'd expect coming from most programming languages. This trips a lot of people up.

#### **test flags**
flag|what is means
-|-
**File tests**
-e|file exists (any type)
-f|exists and is a regular file
-d|exists and is a directory
-L|exists and is a symbolic link
-r|file is readable
-w|file is writable
-x|file is executable
-s|file exists and is not empty (size > 0)
-nt|file is newer than another file (``file1 -nt file2``)
-ot|file is older than another file (``file1 -ot file2``)
**String tests**
-z|string is empty (zero length)
-n|string is not empty
=/==|strings are equal
!=|strings are not equal
**Numeric comparisons**
-eq|equal
-ne|not equal
-gt|greater than
-lt|less than
-ge|greater than or equal
-le|less than or equal
**Logical combinators**
!|negation
-a|AND (inside a single `[ ]`, largely deprecated in favor of `&&`)
-o|OR (same caveat — prefer `&&`/`||` between separate `[ ]` tests)

### **read**
`read` takes input — typed by the user, or piped/redirected from somewhere else — and stores it into a variable.
It's how a script stops being a one-way street. Instead of only producing output, a script can pause and react to input, making it interactive or able to process data line by line.

#### example
```bash
read -p "Enter your name: " name
echo "Hello, $name"
```
* `-p` shows a prompt without needing a separate `echo`.
* `-s` hides input (useful for passwords).
* `-a` reads input into an array.
* `-r` prevents backslashes from being interpreted as escape characters — good practice to include almost always.

### **loops**
Loops let a script repeat a block of commands — either a fixed number of times, over a list of items, or until some condition changes.

#### **three main type** 
``` bash
# for — iterate over a list
for file in *.txt; do
    echo "$file"
done

# while — repeat as long as a condition is true
count=1
while [ $count -le 5 ]; do
    echo $count
    count=$((count + 1))
done

# until — repeat as long as a condition is false (opposite of while)
until [ $count -gt 5 ]; do
    echo $count
    count=$((count + 1))
done
```


### **returned values**
very command in Linux returns an exit status (also called a return code) when it finishes — a number that tells you whether it succeeded or failed.Scripts use this to make decisions — did the previous command work or not? Without checking exit status, a script just barrel forward even after something breaks.
#### example
```bash
ls /etc/passwd
echo $?
```
* $? holds the exit status of the last command run — check it immediately, since it gets overwritten by the next command.
* 0 = success.
* Any non-zero value (1–255) = failure, and different numbers can mean different kinds of failure depending on the command.

