# **103.4  Use streams, pipes, and redirects** 
## **redirecting standard IO**
In linux shell we use three sastndartd data stream for input and output.
- STDIN: standard input stream which provide inout for commands.
- STDOUT: standard output stream, which includes the output of commands.
- STDERR: standard error stream,   which includes the error output of commands.
 
 > 0, 1 and 2 indicating STDIN,STDOUT and STDERR

 |Operation | Usage|
 |----------|------|
 |>|Redirects STDOUT to a file(overwrite if exists)|
 |>>|Redirects STDOUT to a file(append if exists)|
 |2>|Redirects STDERR to a file(overwrite if exists)|
 |2>>|Redirects STDERR to a file(append if exists)|
 |&>|Redirects both STDOUT and STDERR to a file(overwrite if exists)|
 |&>>|Redirects both STDOUT and STDERR to a file(append if exists)|
 |<|Redirecrt STDIN from a file|
 |<>|Redirect STDIN from the file and send the STDOUT to it|


> note: It is also posssible to use &0, &1 and &2 to refer to the target of STDIN , STDOUT and STDERR 

### example 

```bash
ls > fiile1 2>&1    #means redirect output to file1 and redirect stderr to same place as stdout(file1)
```

```bash
ls 2>&1 >file1 #Be carfull it means print stderr in cuurrent location of stdout(terminal) and then change the stdout to file1
```


### **sending to null**
In linux we have  `/dev/null`  works like abyss, if you send somthing in it, it desappears.
### example
```bash 
$ ls j* x* > file1
ls: x*: No such file or directory
$ ls j* x* > file1 2>/dev/null
$ cat file1
jack
```

### **here-documents**
Many shels have here-documents as a way of input,. You use `<<` and a `WORD` and then whatever you input is considered stdin till you give only the `WORD` in a line.
``` bash 
tr ' ' '.' << END_OF_DATA
> this is a line
> and then this
>
> we'll still type
> and,
> done!
> END_OF_DATA
this.is.a.line
and.then.this

we'll.still.type
and,
done!
```

## **Pipes**
With the pipe (`|`) ypu can redirect data stream between multiple commands all on one command line. When you do `command1 | command2`, `command1` executed and result(output) will be ass input of command2.
### example
```bash
ls |grep "\.pdf$" #it'll find pdf file of corrent directory
```

## **xargs**
It used in this pattern: `command1 | xargs command2` reads STDIN and splits it into the arguments append them to `command2`

### example 
```bash
echo "file1.pdf file2.txt" | xargs rm   # rm doesn't read STDIN it expect arguments
```

## **tee**
If you need to see the output and also save it in a file `tee` solve that.The `-a` switch will append to filesif they exist.
>note: if you need save STDERR to, first you need to redurect it to STDOUT 
### example
```bash
ls -1 | tee list_of_my_files    # it'll show the list of my files in terminal and also save it in "list_of_my_files"
```

# **103.5 Create, monitor and kill processes**

## **managing processes**
### **forgrounde and background jobs**
Normaly when we run a program it blocks whole terminal while it's running whe can use `ctrl + c` to kill(breake) it and `ctrl + z` to puss(suspend) it. A stopped job (normaly the last one) can be brought to forground using `fg` you can also use `jobs` to see all jobs and bring to forgraound specific job using job id.
```bash
$ xeyes
^Z
[1]+  Stopped                 xeyes
$ jobs
[1]+  Stopped                 xeyes
$ bg
[1]+ xeyes &
$ jobs
[1]+  Running                 xeyes &
$ sleep 1000 &
[2] 7395
$ jobs
[1]-  Running                 xeyes &
[2]+  Running                 sleep 1000 &
$ fg %2
sleep 1000
^Z
[2]+  Stopped                 sleep 1000
$ jobs
[1]-  Running                 xeyes &
[2]+  Stopped                 sleep 1000
$ bg sle
[2]+ sleep 1000 &
$ jobs
[1]-  Running                 xeyes &
[2]+  Running                 sleep 1000 &
```
>note: `+` at side of job id means if you excute `fg` command it runes that job by default

## **nohup**
If you have some jobs in background and you close terminal your jobs will be terminated. Sometimes you want to run your jobs independent to your terminal in thaht case we use `nohup` command.
### example
```bash
$ nohup ping 4.2.2.4
nohup: ignoring input and appending output to ‘nohup.out’
^C
$ cat nohup.out
PING 4.2.2.4 (4.2.2.4) 56(84) bytes of data.
64 bytes from 4.2.2.4: icmp_seq=1 ttl=51 time=225 ms
64 bytes from 4.2.2.4: icmp_seq=3 ttl=51 time=223 ms

--- 4.2.2.4 ping statistics ---
4 packets transmitted, 2 received, 50% packet loss, time 3010ms
rtt min/avg/max/mdev = 223.584/224.767/225.950/1.183 ms
```
>note:It is common to use 2> to redirect the nohup errors to another file and use a & to run it in the background: nohup script.sh > mynohup.out 2>&1 &


## **kill**
We can breake processes using kill signalls.By default `kill` sends `15` signal which is TERM and tell the process to terminate itself.
### syntax
```bash
kill -SIGNAL_ID_OR_NAME %process_id
```

|signal number(id)| signal name| meaninig|
------------------|------------|---------|
1|HUP|Informing the process that its controling the terminal (like a ssh connection) is terminatted|
15|TERM|normal terminate request|
9|KILL|forcefully kills the process

### **kilall**
Will send the given signal to all process with given name

### **pkill**
Will send the given signal to all process with specific name pattern

## **Monitoring Processes**
### **ps**
The `ps` command shows running processes on your computer. Each process has a process ID(PID) and a parent process ID(PPID). `ps ef` will show you ALL process on a system.
>note: it's also possible to use the `--sort` to sort the output based on different fields

### example
```bash
ps -af --sort +comm,-sid
UID        PID  PPID  C STIME TTY          TIME CMD
root      5486  5478  0 19:59 pts/12   00:00:00 -su
root      4444  1169  0 19:56 tty4     00:00:00 -bash
user1      6638  5412  0 20:10 pts/0    00:00:04 node /usr/local/bin/sslocal
user1      7778  7651  0 20:58 pts/1    00:00:00 ps -af --sort +comm,-sid
user1      7678  7651  0 20:48 pts/1    00:00:00 sleep 1000
user1      7679  7651  0 20:48 pts/1    00:00:00 sleep 1001
user1      7775  7651  0 20:58 pts/1    00:00:00 sleep 1000
user1      7776  7651  0 20:58 pts/1    00:00:00 sleep 1000
user1      7777  7651  0 20:58 pts/1    00:00:00 sleep 1000
root      5478  5477  0 19:59 pts/12   00:00:00 su -
root      5477  5008  0 19:59 pts/12   00:00:00 sudo su -
user1      7680  7651  0 20:48 pts/1    00:00:01 xeyes
```

### **pgrep**
searches for running processes by name (or other attributes) and prints their PIDs. It's basically "grep for processes."

### **top**
Itt's a tool for simple monitoring of system,It will update the status.
You can see the processes, system load, uptime, CPU status, memory, ... and do some stuff:

|key during top|functionality|
|--------------|-------------|
|h|help|
|q|quit|
M|sortbased on memory
c|show full commands
k|kill after asking PID an signal

### **free**
The `free` command will show youy info about the system memory. The default is kilobytes but you can change it with `-m` for megabyte and `-g` for gigabyte or `-b` for byte. UYou can also use `-h` for human readable.

### **uptime**
It shows the time, system uptime, how many users loged in and and the load average.

>note:Although it's one of the most important KPIs of the system status, some of the experienced Linux admins do not know what the load average means. The load average shows how many processes are in the to be run queue. If this number is higher than the number of your CPU cores, you are in a bad situation. If it's close to the number of your cores constantly, it's kind of dangerous, and if it's less than 1/10th of your core numbers, your system is kind of idle. Do you remember how to check the number of your cores? Its in /proc/cpuinfo or nproc.


### **watch**
uns a command repeatedly and shows the live-updating output on screen — useful for monitoring something that changes over time.

|switches | usage|
|---------|------
-n| To specify interval in seconds
-b| Beep if command has a non-zero exxit
-d| Shows the difference between runs

## **Terminal Multiplexers**
### **screen**
If you dont have access to GUI(on a server) and you need to have multiple terminal and run different programs you need tools to multiplex your terminal.One of theme is `screen`.
Run it with `screen` and press `enter` to exit the welcome window into a prompt. You can use it as a normal terminal and detach from it (and let it run in the background) using the Ctrl + A and then D keys. Check the list of your screens with `screen -ls` and re-attach to any of them with `screen -r screen-id`.

Below you can see a few common switches, they all should be issued after the Ctrl + A combination.
|KEY|USAGE|
|---|-----|
\\ |Kill all processes windows and terminate the screen
|\| |Split current window into two vertical focuses
`shift + S` |Split current window into two horizontal focuses
C| Create a window in the current focus
`Tab` | Go to the next focus
D | Detach from window
K |Kill current window
N | Move to Next window
P |Move to the Previous window

> note: A great point about screen (and tmux) is the fact it remains running even after you logout of the system and its possible to relogin and re-attach to the same screen (or tmux)

### **tmux**
> I must dive deeper about tmuxx in a specific curse

# **103.6 Modify process execution priorities**

## **Setting Nice-ness**
Nice-ness of a process is how much it will ask for CPU it's a number between -20 and 19. -20 is so Angry and askes very much CPU and 19 is very nice and don't get so much CPU.
For change niceness level of a program we use `nice` and `-n` switch:
```bash 
nice -n 19 echo "Im running nicely"
```
> Note: Only the root can give niceness value blow 0

## **change priorities**
The `renice` command can be used to change the niceness of your running processes (or others if you are root):

```bash
$ ps -ef | grep firefox
user1     13605 11226 30 08:28 ?        00:10:13 /usr/lib/firefox/firefox
user1     15192 15044  0 09:01 pts/29   00:00:00 grep firefox
$ sudo renice -n -10 13605
13605 (process ID) old priority 5, new priority -10
```

>Note: You can also press `r` in `top` command to renice a process



# **103.7 Search text file using regular expressions**
## **Regex**
Regular expression is a pattern to describe what you want to match from a text.

### repeating
- `*` means repeat previous character 0 time or more.
- `+` means repeat previous character 1 time or more.
- `?` means zero or more repeate.
- `{m,n}` means item needs to match at least `n` times but at most `m` times.

### Alternatiuon
- `a|b` it means `a` or `b`.

### character classes 
- `.` means any character.

- `[:alpha:]` alphabetic characters. In the "C" locale, this is exactly `[A-Za-z]`
- `[:digit:]` decimal digits `0-9`
- `[:alnum:]` union of [:alpha:] and [:digit:]
- `[:upper:]` Uppercase letters
- `[:lower:]` lowercase letters
- `[:blank:]` `space` and `tab` character
- `[:punct:]` printable characters that are neither alphanumeric nor space

### Ranges

- `[a-z]` any lowercase letter a through z
- `[A-Z]` any uppercase letter A through Z
- `[0-9]` any digit 0 through 9
- `[a-zA-Z]` any letter, upper or lower (ranges can be combined)
- `[a-zA-Z0-9]` any letter or digit (alphanumeric)

### matching at specific locations
- `^` caret means beginning of the string.
- `$` dollar means the end of the string.

### example

|REGEX|will match| note
|-----|----------|-----
a*b|aaaab, b, ab,|zero or more `a` then a `b`
a+b|ab, aaab, aaaaaab| needs aat least one `a`
a?b|b, ab| zero or one `a` only
a{2,4}b|aab, aaab, aaaab|needs 2 to 4 `a` before `b`
cat\|dog | cat, dog, I have a cat| matches either word anywhere in text
c.t | cat, cot, cut, c9t| `.` stands for any single character
[[:alpha:]]+ | abc (from "abc123")| matches only the letters
[[:digit:]]+ | 123 (from "abc123")| matches only the digits
[[:alnum:]]+ | abc, 123 (from "abc-123")| letters+digits, but `-` breaks the match
[[:upper:]]+ |H, W (from "Hello World") |only uppercase runs
[[:punct:]]| !, ? (from "Hi! you?") | punctuation, not letters/digits/space
[a-z]+| ello (from "Hello123") | lowercase only, capital H excluded
[A-Z]+|	H (from "Hello123")	|uppercase only
[0-9]+|	123 (from "Hello123")	|digits only
[a-zA-Z]+|	Hello (from "Hello123")	|any letter, upper or lower
[a-zA-Z0-9]+|	Hello, 123 (from "Hello_123!")	|letters+digits, but _ and ! break it
^Error|	Error: file not found	|must be at the very start of the line
done$|	task done	|must be at the very end of the line
^[0-9]+$|	12345	|entire line must be digits, start to end
^$|	(empty line)| matches only a completely empty line

## **grep**

generalised regular expression processor(grep) is a tool we use to search inside files or input stream to find matches.

common switches:
|switch|meaning|
|------|-------|
-c|just show the count
-v|reverse the search
-n|show line numbers
-l|show only file names
-i|case insensitive
-r|Read all files under each directory, recursively

### **Extended grep**
Plain `grep` uses BRE, where several metacharacters are either unavailable or need a backslash to become "special." Extended grep (grep `-E`, same as `egrep`) makes them native — no backslash needed.


### **Fixed grep**
when we search fo exact things not a patern or regex we use `grep -f` or `fgrep`.

# **103.8 Basic file editing**
## **vi mode**
`vi` is simpler version of `vim` that is a powerfull text editor and that works in two mode: `isert` and `command`

1. command: when we just open `vi` or `vim` it's on `command` mode where you gho aroun the file, search, delete text, copy past and .... some commadns are start with `:` and others are only keypress.
2. insert: from `command` mode we go to insert with pressing `i` and we can write in file and edite file.
> Note: to switch to command jsut  press `esc`.

### **moving cursor**
To move around a text file, use these keys in Command mode:
|key|function|
|---|--------|
h | One Cahracter to left(only same line)|
j| one line down
k| one line up
l | one charecter to right
w| next word on the current line
e| Next end of word on the current line
b|Previous beginning of the word on the current line
ctrl-f| scrol forward
ctrl-b| scrol backward
 > Note: Typing a number before most commands will repeat the command that many times. i.e `5j` means 5 line down.

### **jumping around**
|key|function|
|---|--------|
G| With no number, will jump to the end & 10G will jump to line 10
H|5H will go to the 5th line from the top of the screen
L|3L will move the cursor to the 3rd line to the last line of the screen


### **Editing text**
These command during the command mode will help you enter, edit, replace and text:
|key|function|
|---|--------|
i|Enter the insert mode
a|Enter the insert mode after the current position of the cursor
r|replace only one character
o|open a new line below the cursor and go to the insert mode
O|open a new line above the cursor and go to the insert mode
c|clear to a location and go to the insert mode the replace till there and then normal insert(`cw` will overwrite the current word)
d| delete. you can mix with w (`dw`) to delete a word.
dd|Delete the current line
x|Delete character at the position of the cursor
p|Paste the last deleted text after the cursor
P|Paste the last deleted text before the cursor
xp|swaps the character at the cursor position with the one on its right

### **Searching**
|key|function|
|---|--------|
/|Search forward (`/hello` will find the next hello)
?|Search backward
n|repeat previous search. (You can also use `/` and `?` without any parameters)

### **Exiting**
Istedd of rebot system :))))) we can use those commands:

|key|function|
|---|--------|
:q!| Quit editing without saving = runaway after any mistake
:w!|Write the file (whether modified or not). Attempt to overwrite existing files or read-only or other unwritable files
:w newfile.txt| Write to a new name
ZZ:Exit and save the file if modified
:e!|Reload the file from disk
:!|Run a shell command

