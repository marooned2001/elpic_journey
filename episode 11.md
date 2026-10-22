# **107.1 Manage user and group accounts and realted system files**
## **password**
### changing password
every user can change the password using `passwd` command.root user can change every user's password using `passwd [username]`.

## **Users and groups**
linux can handle multi user and a linux system admin must can add, remove, and modify other users.

Also linux has the consept of groups. we can give same privlages to a group of users.
### **managing users**
#### **adding users**
We use `useradd` command.
|switches|meaning|
|-|-|
-d| home directory
-m| create the home directory
-s| specify shell
-G| add aditional group
-c|comment. most of the time, users actual name. Use quotes if comments has spaces or special characters in them


#### **modifying users**
We can modify users using `usermod` command. `useradd` switches are working here to but we have three additional.
switches|meaning
---|--
-L|lock this account
-U|Unlock the account
-aG|append to more groups


#### **deleting users**
we use `userdel [username]` command to delet users.
> Note: if we add `-r` switch the home directory and mail spool will be erased too!

### **managing groups**
we use `groupadd` command to create new group, and use `groupdel` to delet it and `groupmod` to modify.

## **Files related to users and groups.**

### `/etc/passwd`
One line per user, world-readable. Fields: `name:x:UID:GID:comment:home:shell`
>Note1: As you can see, the 2nd field is shown as "x" here. In the old days the password or the hashed password of the user was shown in this file but nowadays that is moved to the `/etc/shadow` file.

>Note2: how special users like lightdm have `/bin/false` as their shell; this prevents them from logging into the system for real. In old days hackers used to try these accounts to login.

### `/etc/shadow`
Password hashes and aging info, readable by root only.fields:
`name:hash:last_change:min:max:warn:inactive:expire:reserved`

> Note: `chage`(change password aging)manages password aging and account expiry. It reads and writes the aging fields in `/etc/shadow`, so you normally need root (a regular user can only view their own info with `-l`).

### `/etc/groups`
This file contains the groups and their IDs.

# **107.2 Automate system administration task by scheduling jobs**
## **cron**
We can set a task to run in a specsific period using `cron`.
The `crond` daemon wakes up every minute and checks whether any job is due.
for setting a task using `cron` we must add it to `crontab`.
we can edit it using `crontab -e` and we can see the list of our crons using `crontab -l`.
Each crontab configuration line has 6 fields. The first 5 specify a specific interval and whatever after than is used as the "command".
```
┌──────── minute        (0-59)
│ ┌────── hour          (0-23)
│ │ ┌──── day of month  (1-31)
│ │ │ ┌── month         (1-12 or jan-dec)
│ │ │ │ ┌ day of week   (0-7, 0 and 7 = Sunday, or sun-sat)
│ │ │ │ │
* * * * *  command
```
### **system wide cron**
There is file called `/etc/crontab.` This looks like a normal user file opened with `crontab -e` but has one extra filed `user`.
we can mention which user runs this commands.

## **at**
we use `at` to run a jobs once at a specific time.
### example
```
at 10:30                  # then type commands, finish with Ctrl+D
at 10:30 tomorrow
at now + 5 minutes
at 14:00 12/25/2026
at teatime                # 16:00
at -f script.sh 23:00     # run a file at 23:00
```
### **Managing at jobs**
Command|Action
-|-
atq (or at -l)|List pending jobs
atrm 7 (or at -d 7)|Remove job number 7
at -c 7|Show the contents of job 7
batch|Run a job when the system load is low

## **managing cron access**
We can manage `cron` and `at` access using `/etc/cron.allow` , `/etc/cron.deny`, `/etc/at.allow` and `/etc/at.deny`.

**In most systems none of these files exist but if you create them, they will become active as follow:**
file extension|functionality
-|-
.allow|ONLY users mentioned in this file are allowed to run this service. All other users will be denied
.deny|Everybody can use the service except the users mentioned in this file

## **systemd**
A timer is a unit that activates a matching service unit on a schedule. You need two files:
`/etc/systemd/system/job.service`
```
[Unit]
Description=Nightly backup

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup.sh
```
`/etc/systemd/system/backup.timer`
```
[Unit]
Description=Run backup nightly

[Timer]
OnCalendar=*-*-* 02:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

Activate it:
```bash
systemctl daemon-reload
systemctl enable --now backup.timer
systemctl list-timers          # show all timers and next run times
```
Timer types:
Option|meaning
-|-
OnCalendar=|Real-time schedule (like cron): `daily`, `weekly`, `Mon *-*-* 04:30`
OnBootSec=| Time after boot
OnUnitActiveSec=|Time after the service last ran (repeating interval)
Persistent=true|Run missed jobs after downtime (like anacron)

# **107.3 Localisation and internationalisation**
## **timezone**
On linux systems you can use `date` commands to check the date and the calendar. It is possible to print a custom date using `+` formatter.
```bash
date
# Thu Oct 22 14:30:05 EDT 2026

date +%Y
# 2026

date +%Y-%m-%d
# 2026-10-22
```

code|meaning|example
-|-|-
%Y|Year, 4 digits|2026
%y|Year, 2 digits|26
%m|Month number|10
%B|Month name|October
%d|Day of month|22
%A|Weekday name|Thursday
%H|Minute|30
%S|Second|05
%s|Seconds since 1970-01-01|1792679405

### Mixing codes with your own text
```bash
date "+Today is %A, %d %B %Y"
# Today is Thursday, 22 October 2026

date "+%H:%M"
# 14:30
```

to get more info you can try `timedatectl`
```bash
mmd@mmd-VMware-Virtual-Platform:~$ timedatectl 
               Local time: Thu 2026-10-22 17:46:38 +0330
           Universal time: Thu 2026-10-22 14:16:38 UTC
                 RTC time: Thu 2026-10-22 14:16:37
                Time zone: Asia/Tehran (+0330, +0330)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```

You can change timezone using `tzselect`
```bash
mmd@mmd-VMware-Virtual-Platform:~$ tzselect 
Please identify a location so that time zone rules can be set correctly.
Please select a continent, ocean, "coord", or "TZ".
 1) Africa
 2) Americas
 3) Antarctica
 4) Asia
 5) Atlantic Ocean
 6) Australia
 7) Europe
 8) Indian Ocean
 9) Pacific Ocean
10) coord - I want to use geographical coordinates.
11) TZ - I want to specify the timezone using the Posix TZ format.
#? 4
Please select a country whose clocks agree with yours.
 1) Afghanistan		     30) Kyrgyzstan
 2) Antarctica		     31) Laos
 3) Armenia		     32) Lebanon
 4) Australia		     33) Macau
 5) Azerbaijan		     34) Malaysia
 6) Bahrain		     35) Mongolia
 7) Bangladesh		     36) Myanmar (Burma)
 8) Bhutan		     37) Nepal
 9) Brunei		     38) Oman
10) Cambodia		     39) Pakistan
11) China		     40) Palestine
12) Christmas Island	     41) Philippines
13) Cocos (Keeling) Islands  42) Qatar
14) Cyprus		     43) Réunion
15) East Timor		     44) Russia
16) French S. Terr.	     45) Saudi Arabia
17) Georgia		     46) Seychelles
18) Hong Kong		     47) Singapore
19) India		     48) Sri Lanka
20) Indonesia		     49) Syria
21) Iran		     50) Taiwan
22) Iraq		     51) Tajikistan
23) Israel		     52) Thailand
24) Japan		     53) Turkmenistan
25) Jordan		     54) United Arab Emirates
26) Kazakhstan		     55) Uzbekistan
27) Korea (North)	     56) Vietnam
28) Korea (South)	     57) Yemen
29) Kuwait
#? 21

The following information has been given:

	Iran

Therefore TZ='Asia/Tehran' will be used.
Selected time is now:	Thu Oct 22 17:48:28 +0330 2026.
Universal Time is now:	Thu Oct 22 14:18:28 UTC 2026.
Is the above information OK?
1) Yes
2) No
#? 1

You can make this change permanent for yourself by appending the line
	TZ='Asia/Tehran'; export TZ
to the file '.profile' in your home directory; then log out and log in again.

Here is that TZ value again, this time on standard output so that you
can use the /usr/bin/tzselect command in shell scripts:
Asia/Tehran
```

## **Configuring Languages**
A locale is a set of rules for a language and region: message language, date and time format, number format (`1,000.5` vs `1.000,5`), currency, and sorting order.
To check the status of current selected system language, use the `locale` command:
```bash
mmd@mmd-VMware-Virtual-Platform:~$ locale
LANG=en_US.UTF-8
LANGUAGE=
LC_CTYPE="en_US.UTF-8"
LC_NUMERIC="en_US.UTF-8"
LC_TIME="en_US.UTF-8"
LC_COLLATE="en_US.UTF-8"
LC_MONETARY="en_US.UTF-8"
LC_MESSAGES="en_US.UTF-8"
LC_PAPER="en_US.UTF-8"
LC_NAME="en_US.UTF-8"
LC_ADDRESS="en_US.UTF-8"
LC_TELEPHONE="en_US.UTF-8"
LC_MEASUREMENT="en_US.UTF-8"
LC_IDENTIFICATION="en_US.UTF-8"
LC_ALL=
```
### The environment variables
Variable|Controls
-|-
LANG | The default locale for everything
LC_ALL | Overrides everything, including all `LC_*` and `LANG`
LC_MESSAGES | Language of program messages
LC_TIME | Date and time format
LC_NUMERIC | Number format
LC_MONETARY |	Currency format
LC_COLLATE	| Sorting order
LC_CTYPE |	Character classification (upper/lower case, which characters are letters)

### LANG=C
`LANG=C` is the minimal, locale-neutral setting: plain ASCII, English messages, and simple byte-order sorting. It is often used in scripts so output is predictable on every machine.The `LANG=C` settings is used in many programs and scripts to make sure that the system will create coherent and predictable results in all environments. For example to make sure that the sort order will be in Binary order and all language settings will be default (en.US).

## **Character Encoding**
### **ACSII**
Computers used to work with 7bit characters encoding. That would give us 128 characters which was enough for numbers, punctuation and digits!
### **ISO-8859**
It had more characters and a lots of sets for Thai, Arabic and other languages but still had ASCII character sets.
### **UTF-8**
The `Unicode Transformation Format` is the newest encoding method. It is a real universal encoding with characters not only for all written languages but also for fun characters like ¾, ♠, π and ⚤. It is backward compatible with the ASCII and uses 8 bit code units (not 8 bit coding!). In most cases it is a good idea to use UTF-8 and be sure that your system will work in practically all cases.
### **iconv**
If you needed to convert coding to each other, the command is `iconv`. The `-l` switch will show you all the available codings