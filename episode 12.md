# **108.1 Maintain system time**
## **hardware clock**
there is a clock in hard ware named RTC(real time clock) It has its own battery and keeps the time even when the computer is off. The OS reads the clock from it on boot time and sets ysytem clock based on it.
We can see itt using `hwclock` command.
```bash
sudo hwclock 
2026-10-31 18:06:42.992870+03:30
```
>Note: -w tells the hwclock to set the hardware time based on the current system time.

## **Timezone**
We have two important file `/etc/timezone` and `/etc/localtime`.

### */etc/timezone*
It is a text file, representing your timezone. This is used if a program wants to know about your time zone of show it.
```bash
mmd@mmd-VMware-Virtual-Platform:~$ file /etc/timezone 
/etc/timezone: ASCII text
mmd@mmd-VMware-Virtual-Platform:~$ cat /etc/timezone 
Asia/Tehran
```

### */etc/localtime*
It is a binary file describing your timezone info to the system.
```bash
mmd@mmd-VMware-Virtual-Platform:~$ file /etc/localtime 
/etc/localtime: symbolic link to /usr/share/zoneinfo/Asia/Tehran
mmd@mmd-VMware-Virtual-Platform:~$ file /usr/share/zoneinfo/Asia/Tehran 
/usr/share/zoneinfo/Asia/Tehran: timezone data (fat), version 2, no gmt time flags, no std time flags, no leap seconds, 72 transition times, 8 local time types, 28 abbreviation chars
mmd@mmd-VMware-Virtual-Platform:~$ od -c /usr/share/zoneinfo/Asia/Tehran 
0000000   T   Z   i   f   2  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
0000020  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
0000040  \0  \0  \0   H  \0  \0  \0  \b  \0  \0  \0 034 232   l   } 310
0000060 277  \0 314   H  \r 224   D   8 016 255 023 270 017   y   s   @
0000100 020   ( 312 300 020 251 375 300 021 254   j 310 022   E   J 270
0000120 023   7 354 310 024   - 025 270   (       v 310   ( 333 235 270
0000140   ) 313 234 310   * 276   " 270   + 254 320   H   , 237   V   8
0000160   - 216 003 310   . 200 211 270   /   o   7   H   0   a 275   8
0000200   1   P   j 310   2   B 360 270   3   2 357 310   4   %   u 270
0000220   5 024   #   H   6 006 251   8   6 365   V 310   7 347 334 270
0000240   8 326 212   H   9 311 020   8   : 271 017   H   ; 253 225   8
0000260   < 232   B 310   = 214 310 270   >   {   v   H   ?   m 374   8
0000300   @   \ 251 310   A   O   / 270   B   ?   . 310   C   1 264 270
0000320   G 342 311   H   H 325   O   8   I 305   N   H   J 267 324   8
0000340   K 246 201 310   L 231  \a 270   M 207 265   H   N   z   ;   8
0000360   O   h 350 310   P   [   n 270   Q   K   m 310   R   = 363 270
0000400   S   , 241   H   T 037   '   8   U  \r 324 310   V  \0   Z 270
0000420   V 357  \b   H   W 341 216   8   X 321 215   H   Y 304 023   8
0000440   Z 262 300 310   [ 245   F 270   \ 223 364   H   ] 206   z   8
0000460   ^   u   ' 310   _   g 255 270   `   W 254 310   a   J   2 270
0000500   b   8 340   H   c   +   f   8 177 377 377 377 001 003 002 005
0000520 004 005 003 002 003 002 003 002 003 002 003 002 003 002 003 002
0000540 003 002 003 002 003 002 003 002 003 002 003 002 003 002 003 002
*
0000620 003 002 003 003  \0  \0   0   8  \0  \0  \0  \0   0   8  \0 004
0000640  \0  \0   ?   H 001  \b  \0  \0   1   8  \0 016  \0  \0   F   P
0000660 001 024  \0  \0   8   @  \0 030  \0  \0   ?   H 001  \b  \0  \0
0000700   1   8  \0 016   L   M   T  \0   T   M   T  \0   +   0   4   3
0000720   0  \0   +   0   3   3   0  \0   +   0   5  \0   +   0   4  \0
0000740   T   Z   i   f   2  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
0000760  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
0001000  \0  \0  \0   H  \0  \0  \0  \b  \0  \0  \0 034 377 377 377 377
0001020 232   l   } 310 377 377 377 377 277  \0 314   H  \0  \0  \0  \0
0001040  \r 224   D   8  \0  \0  \0  \0 016 255 023 270  \0  \0  \0  \0
0001060 017   y   s   @  \0  \0  \0  \0 020   ( 312 300  \0  \0  \0  \0
0001100 020 251 375 300  \0  \0  \0  \0 021 254   j 310  \0  \0  \0  \0
0001120 022   E   J 270  \0  \0  \0  \0 023   7 354 310  \0  \0  \0  \0
0001140 024   - 025 270  \0  \0  \0  \0   (       v 310  \0  \0  \0  \0
0001160   ( 333 235 270  \0  \0  \0  \0   ) 313 234 310  \0  \0  \0  \0
0001200   * 276   " 270  \0  \0  \0  \0   + 254 320   H  \0  \0  \0  \0
0001220   , 237   V   8  \0  \0  \0  \0   - 216 003 310  \0  \0  \0  \0
0001240   . 200 211 270  \0  \0  \0  \0   /   o   7   H  \0  \0  \0  \0
0001260   0   a 275   8  \0  \0  \0  \0   1   P   j 310  \0  \0  \0  \0
0001300   2   B 360 270  \0  \0  \0  \0   3   2 357 310  \0  \0  \0  \0
0001320   4   %   u 270  \0  \0  \0  \0   5 024   #   H  \0  \0  \0  \0
0001340   6 006 251   8  \0  \0  \0  \0   6 365   V 310  \0  \0  \0  \0
0001360   7 347 334 270  \0  \0  \0  \0   8 326 212   H  \0  \0  \0  \0
0001400   9 311 020   8  \0  \0  \0  \0   : 271 017   H  \0  \0  \0  \0
0001420   ; 253 225   8  \0  \0  \0  \0   < 232   B 310  \0  \0  \0  \0
0001440   = 214 310 270  \0  \0  \0  \0   >   {   v   H  \0  \0  \0  \0
0001460   ?   m 374   8  \0  \0  \0  \0   @   \ 251 310  \0  \0  \0  \0
0001500   A   O   / 270  \0  \0  \0  \0   B   ?   . 310  \0  \0  \0  \0
0001520   C   1 264 270  \0  \0  \0  \0   G 342 311   H  \0  \0  \0  \0
0001540   H 325   O   8  \0  \0  \0  \0   I 305   N   H  \0  \0  \0  \0
0001560   J 267 324   8  \0  \0  \0  \0   K 246 201 310  \0  \0  \0  \0
0001600   L 231  \a 270  \0  \0  \0  \0   M 207 265   H  \0  \0  \0  \0
0001620   N   z   ;   8  \0  \0  \0  \0   O   h 350 310  \0  \0  \0  \0
0001640   P   [   n 270  \0  \0  \0  \0   Q   K   m 310  \0  \0  \0  \0
0001660   R   = 363 270  \0  \0  \0  \0   S   , 241   H  \0  \0  \0  \0
0001700   T 037   '   8  \0  \0  \0  \0   U  \r 324 310  \0  \0  \0  \0
0001720   V  \0   Z 270  \0  \0  \0  \0   V 357  \b   H  \0  \0  \0  \0
0001740   W 341 216   8  \0  \0  \0  \0   X 321 215   H  \0  \0  \0  \0
0001760   Y 304 023   8  \0  \0  \0  \0   Z 262 300 310  \0  \0  \0  \0
0002000   [ 245   F 270  \0  \0  \0  \0   \ 223 364   H  \0  \0  \0  \0
0002020   ] 206   z   8  \0  \0  \0  \0   ^   u   ' 310  \0  \0  \0  \0
0002040   _   g 255 270  \0  \0  \0  \0   `   W 254 310  \0  \0  \0  \0
0002060   a   J   2 270  \0  \0  \0  \0   b   8 340   H  \0  \0  \0  \0
0002100   c   +   f   8  \0  \0  \0  \0 177 377 377 377 001 003 002 005
0002120 004 005 003 002 003 002 003 002 003 002 003 002 003 002 003 002
0002140 003 002 003 002 003 002 003 002 003 002 003 002 003 002 003 002
*
0002220 003 002 003 003  \0  \0   0   8  \0  \0  \0  \0   0   8  \0 004
0002240  \0  \0   ?   H 001  \b  \0  \0   1   8  \0 016  \0  \0   F   P
0002260 001 024  \0  \0   8   @  \0 030  \0  \0   ?   H 001  \b  \0  \0
0002300   1   8  \0 016   L   M   T  \0   T   M   T  \0   +   0   4   3
0002320   0  \0   +   0   3   3   0  \0   +   0   5  \0   +   0   4  \0
0002340  \n   <   +   0   3   3   0   >   -   3   :   3   0  \n
0002356
```

## **NTP**
Network Time Protocol (NTP) synchronizes clocks over the network using a hierarchy of strata:
* Stratum 0: reference clocks (GPS, atomic clocks), not on the network
* Stratum 1: servers directly attached to stratum 0
* Stratum 2: sync from stratum 1, and so on, up to stratum 15
* Stratum 16: unsynchronized

NTP uses UDP port 123. It prefers to slew the clock (speed it up or slow it down gradually) rather than step it, so time never jumps backwards. If the offset is large (above about 128 ms), it steps the clock.
One of the most famous severs used by ntp people is `pool.ntp.org`. If you check that website you will see that it is a pool of ntp servers and by giving your NTP server the `pool.ntp.org`, it will be redirected to one of the many ntp servers available on that pool.

### *ntpdate*
`ntpdate` queries one or more NTP servers once, sets the system clock, and exits. It does not run as a daemon and does not keep correcting drift.
```bash
mmd@mmd-VMware-Virtual-Platform:~$ sudo ntpdate pool.ntp.org
[sudo] password for mmd: 
2026-10-31 22:47:47.66282 (+0330) +0.215849 +/- 0.382462 pool.ntp.org 44.27.36.97 s1 no-leap
```

### *ntpd*
Instead of manually setting the time each time, you can use a linux service called ntp to keepyour time using some time servers
```bash
sudo apt install ntp #install ntp 
sudo systemctl start ntp #start the server
```
### *ntpq*
The `ntpq` queries the `ntp` service.For print them we should use `-p`.
```bash 
md@mmd-VMware-Virtual-Platform:~$ ntpq -p
     remote                                   refid      st t when poll reach   delay   offset   jitter
=======================================================================================================
 0.ubuntu.pool.ntp.org                   .POOL.          16 p    -   64    0   0.0000   0.0000   0.0076
 1.ubuntu.pool.ntp.org                   .POOL.          16 p    -   64    0   0.0000   0.0000   0.0076
 2.ubuntu.pool.ntp.org                   .POOL.          16 p    -   64    0   0.0000   0.0000   0.0076
 3.ubuntu.pool.ntp.org                   .POOL.          16 p    -   64    0   0.0000   0.0000   0.0076
-185.125.190.58                          29.88.99.4       2 u  231  256  377 319.3843 -134.543 123.1886
*24.249.17.252                           96.147.147.69    2 u    7  256  377 243.5008  12.4086 151.1913
+172.232.15.202                          82.35.162.146    2 u  244  256  377 284.6981 -12.6231 109.5685
+23.143.82.48                            35.73.197.144    2 u   24  256  377 355.0705  38.1778 299.2076
+23.186.168.132                          132.163.96.6     2 u  158  256  377 304.0435  -8.7023 146.5911
+104.131.155.175                         206.213.36.222   2 u   32  256  377 336.6453  -9.9324 116.7718
+23.155.72.147                           69.89.207.99     2 u    6  256  377 267.2178  13.7127 100.1915
+45.77.126.122                           215.243.202.191  2 u  198  256  377 307.9733  18.7663 170.3265
+162.254.225.151                         172.23.17.113    2 u  170  256  377 260.5412   2.6443 134.0832
+23.157.160.168                          129.6.15.28      2 u  148  256  337 268.9532 -20.4121 207.3311
+66.228.59.187                           168.239.11.197   2 u  156  256  377 286.3578  -5.2289 145.5027
```

## **chrony**
chrony is a modern implementation of NTP.  Compared to `ntpd` , this tool provides better results at synchronize time difficult conditions such as intermittent network connections (such as laptops) and congested networks.   
```bash
mmd@mmd-VMware-Virtual-Platform:~$ cat /etc/chrony/chrony.conf 
# Welcome to the chrony configuration file. See chrony.conf(5) for more
# information about usable directives.

# Include configuration files found in /etc/chrony/conf.d.
confdir /etc/chrony/conf.d

# This will use (up to):
# - 4 sources from ntp.ubuntu.com which some are ipv6 enabled
# - 2 sources from 2.ubuntu.pool.ntp.org which is ipv6 enabled as well
# - 1 source from [01].ubuntu.pool.ntp.org each (ipv4 only atm)
# This means by default, up to 6 dual-stack and up to 2 additional IPv4-only
# sources will be used.
# At the same time it retains some protection against one of the entries being
# down (compare to just using one of the lines). See (LP: #1754358) for the
# discussion.
#
# About using servers from the NTP Pool Project in general see (LP: #104525).
# Approved by Ubuntu Technical Board on 2011-02-08.
# See http://www.pool.ntp.org/join.html for more information.
pool ntp.ubuntu.com        iburst maxsources 4
pool 0.ubuntu.pool.ntp.org iburst maxsources 1
pool 1.ubuntu.pool.ntp.org iburst maxsources 1
pool 2.ubuntu.pool.ntp.org iburst maxsources 2

# Use time sources from DHCP.
sourcedir /run/chrony-dhcp

# Use NTP sources found in /etc/chrony/sources.d.
sourcedir /etc/chrony/sources.d

# This directive specify the location of the file containing ID/key pairs for
# NTP authentication.
keyfile /etc/chrony/chrony.keys

# This directive specify the file into which chronyd will store the rate
# information.
driftfile /var/lib/chrony/chrony.drift

# Save NTS keys and cookies.
ntsdumpdir /var/lib/chrony

# Uncomment the following line to turn logging on.
#log tracking measurements statistics

# Log files location.
logdir /var/log/chrony

# Stop bad estimates upsetting machine clock.
maxupdateskew 100.0

# This directive enables kernel synchronisation (every 11 minutes) of the
# real-time clock. Note that it can't be used along with the 'rtcfile' directive.
rtcsync

# Step the system clock instead of slewing it if the adjustment is larger than
# one second, but only in the first three clock updates.
makestep 1 3

# Get TAI-UTC offset and leap seconds from the system tz database.
# This directive must be commented out when using time sources serving
# leap-smeared time.
leapsectz right/UTC
```

# **108.2 System logging**
## **dmesg**
`dmesg` prints the "kernel ring buffer". kernel ring buffer is messeges the kernel generaites during boot and while running.(hardware detection, driver loading, USB plug/unplug, disk errors, OOM kills, firewall drops, and so on).
### Essential options
option | what it does
-|-
dmesg|	Dump the whole buffer 
-T|	Human-readable timestamps (default is seconds since boot)
-H|	Human-friendly output: pager, colors, and relative timestamps
-w|	Follow new messages live, like tail -f
-l| err,warn	Filter by level (--level)
-f| kern,daemon	Filter by facility (--facility)
-c|	Print, then clear the buffer
-C|	Clear the buffer without printing
-n| LEVEL	Set the console log level (which messages print to the console)
-x|	Decode facility and level into readable prefixes
-k|	Show only kernel messages

## **log rotation**
Log files must be managed or will grow for ever.logrotate rotates, compresses, removes, and optionally mails logs on a schedule.

There is two file:
`/etc/logrotate.conf` -> Global defaults, plus an include `/etc/logrotate.d` line,
`/etc/logrotate.d/*` -> Per-application configs (rsyslog, apache, etc.)
>Note: Settings in a per-app file override the global defaults.

```bash
mmd@mmd-VMware-Virtual-Platform:~$ cat /etc/logrotate.conf 
# see "man logrotate" for details

# global options do not affect preceding include directives

# rotate log files weekly
weekly

# use the adm group by default, since this is the owning group
# of /var/log/.
su root adm

# keep 4 weeks worth of backlogs
rotate 4

# create new (empty) log files after rotating old ones
create

# use date as a suffix of the rotated file
#dateext

# uncomment this if you want your log files compressed
#compress

# packages drop log rotation information into this directory
include /etc/logrotate.d

# system-specific logs may also be configured here.

mmd@mmd-VMware-Virtual-Platform:~$ cat /etc/logrotate.d/apt
/var/log/apt/term.log {
  rotate 12
  monthly
  compress
  missingok
  notifempty
}

/var/log/apt/history.log {
  rotate 12
  monthly
  compress
  missingok
  notifempty
}

```

These are the meaning of some of these parameters:
parameters | meaning
-|-
weekly|	rotate logs weekly
missingok|	it is fine if there is no log for this week
rotate 52	|keep the latest 52 logs and delete the older ones
compress|	compress the logs
create 0640 www-data adm|	create the files with this access and owners
pre & post rotate|	run these scripts or commands before and after the rotation

>Note: The logrotate runs using crons and does its job on a daily basis based on a configuration on `/etc/cron.daily/logrotate`

## some famous log files

Generally logs are saved at /var/log. 
lets have a look at some of the famous logs:

#### `var/log/auth.log`(in debian based)
Authentication processes will log here. Things like cron jobs, failed logins, sudo informations, ...

#### `/var/log/syslog`
A centralized place most of the logs received by rsyslogd if a specific logfile is not provided in `/etc/rsyslog.conf`

#### `/var/log/debug`
Debug information from programs.

#### `/var/log/kern.log`
Kernel messages.

#### `/var/log/messages`
Informative messages from services. This is also the default place for logs for remote clients.

#### `/var/run/utmp` & `/var/log/wtmp`
Successful logins.

#### `/var/log/btmp`
Failed logins. You can check this to see if anyone is trying to guess your passwords!

#### `/var/log/faillog`
Failed authentication attempts.

#### `/var/log/lastlog`
Date and time of recent user logins.

### **sevice loges**
Service logs do create files or directories at `/var/log` and update their logs there. For example there might be a `/var/log/apache2/` (or `/var/log/httpd`) for the Apache HTTP server or a `/var/log/mysql` for the MySQL DB.

## **rsyslog**
rsyslog is the syslog daemon on most distros (Debian/Ubuntu, RHEL/CentOS, etc.). It receives log messages from the kernel, programs, and remote hosts, then sorts them into files, the console, or other machines based on rules.
This is the newer generation of syslogs and is used in many environments. Its main configuration is located at `/etc/rsyslog.conf` and it also reads anything in `/etc/rssylog.d/` directory. 
Its configuration consists of 3 main sections:
1. **MODULES**  => rsyslog is modular. Modules are plugins that add inputs (where messages come from) or outputs (where they go).
    * `imuxsock` : receives logs from local programs via `/dev/log`
    * `imklog ` : brings in kernel messages (the dmesg buffer)
    * `immark` : periodic `-- MARK --` lines (commented out, so off)
    * `imudp` / `imtcp` : receive remote logs on port 514 (commented out, so your machine is not a log server)
2. **Global directives** => These start with $, the legacy syntax. They set defaults for the whole daemon.
    * `$RepeatedMsgReduction on` : collapses repeated identical messages into “repeated N times”
    * `$FileOwner` / `$FileGroup` / `$FileCreateMode 0640` : log files are `syslog:adm`, readable by owner and group only.
    * `$DirCreateMode` / `$Umask` : permissions for created directories and the default umask
    * `$PrivDropToUser`/`Group syslog`: starts as root, then drops to the unprivileged `syslog` user
    * `$WorkDirectory` : where queue and state files are kept
3. **Include** => This loads every `.conf` file in `/etc/rsyslog.d/`, in alphabetical order. This is where your `RULES` live, in `50-default.conf`.

### ROULS
 `ROULS` is A combination of facilities, priorities and actions tells the `rsyslog` what to do with each log. In newer versions of linux ROULS are not in `rsyslog.conf` it moved to `50-default.conf`. A roul looks like: `facility.priority    where-to-put-it`
#### EX:
```
  auth.*    /var/log/auth.log
  ```
  It means authantication logs with any priority must be in  `/var/log/auth.log`.

> Note: if we want to send some logs to a remote log server we use `@ip`.

 > Note: The priority could be one of the : emerg/panic, alert, crit, err/error, warn/warning, notice, info or debug. And facilities could be things like: kern, user, mail, daemon, cron, auth, ntp, security, console, syslog, ...

#### **Priority Levels**
level|description
-|-
emerge|   system is unusable
alert|    action must be taken immediately
crit|     critical conditions
error|    error conditions
warning|  warning conditions
notice|   normal, but significant, condition
info|     informational message
debug|    debug-level message
panic|    deprecated synonym for emerg
error|    deprecated synonym for err
warn|     deprecated synonym for warnin

## **logger**
`logger` sends messege to `rstyslog` from command line.
```bash
mmd@mmd-VMware-Virtual-Platform:~$ logger "hello mf"
mmd@mmd-VMware-Virtual-Platform:~$ tail /var/log/syslog
2026-11-17T20:09:23.656655+03:30 mmd-VMware-Virtual-Platform systemd[1]: NetworkManager-dispatcher.service: Deactivated successfully.
2026-11-17T20:10:08.277469+03:30 mmd-VMware-Virtual-Platform systemd[1]: Starting sysstat-collect.service - system activity accounting tool...
2026-11-17T20:10:08.300097+03:30 mmd-VMware-Virtual-Platform systemd[1]: sysstat-collect.service: Deactivated successfully.
2026-11-17T20:10:08.300968+03:30 mmd-VMware-Virtual-Platform systemd[1]: Finished sysstat-collect.service - system activity accounting tool.
2026-11-17T20:15:01.766918+03:30 mmd-VMware-Virtual-Platform CRON[8270]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
2026-11-17T20:17:01.796975+03:30 mmd-VMware-Virtual-Platform CRON[8275]: (root) CMD (cd / && run-parts --report /etc/cron.hourly)
2026-11-17T20:20:03.278208+03:30 mmd-VMware-Virtual-Platform systemd[1]: Starting sysstat-collect.service - system activity accounting tool...
2026-11-17T20:20:03.301910+03:30 mmd-VMware-Virtual-Platform systemd[1]: sysstat-collect.service: Deactivated successfully.
2026-11-17T20:20:03.302930+03:30 mmd-VMware-Virtual-Platform systemd[1]: Finished sysstat-collect.service - system activity accounting tool.
2026-11-17T20:20:34.703157+03:30 mmd-VMware-Virtual-Platform mmd: hello mf
```

## **journald**
systemd-journald is the logging service of systemd. It collects logs from the kernel, services, and programs, and stores them in a binary database (the journal). You read it with `journalctl`.
```bash
mmd@mmd-VMware-Virtual-Platform:~$ systemctl status systemd-journald
● systemd-journald.service - Journal Service
     Loaded: loaded (/usr/lib/systemd/system/systemd-journald.service; static)
    Drop-In: /usr/lib/systemd/system/systemd-journald.service.d
             └─nice.conf
     Active: active (running) since Thu 2026-11-12 13:51:10 +0330; 5 days ago
TriggeredBy: ○ systemd-journald-audit.socket
             ● systemd-journald-dev-log.socket
             ● systemd-journald.socket
       Docs: man:systemd-journald.service(8)
             man:journald.conf(5)
   Main PID: 404 (systemd-journal)
     Status: "Processing requests..."
      Tasks: 1 (limit: 3189)
   FD Store: 98 (limit: 4224)
     Memory: 35.3M (peak: 36.7M)
        CPU: 1.969s
     CGroup: /system.slice/systemd-journald.service
             └─404 /usr/lib/systemd/systemd-journald

Nov 12 13:51:10 mmd-VMware-Virtual-Platform systemd-journald[404]: Collecting audit messages is disabled.
Nov 12 13:51:10 mmd-VMware-Virtual-Platform systemd-journald[404]: Journal started
Nov 12 13:51:10 mmd-VMware-Virtual-Platform systemd-journald[404]: Runtime Journal (/run/log/journal/de98d19e444c43538380c2f1cd33dbaa) is 4.8M, max 38.6M, 33.8M f>
Nov 12 13:51:10 mmd-VMware-Virtual-Platform systemd-journald[404]: Time spent on flushing to /var/log/journal/de98d19e444c43538380c2f1cd33dbaa is 5.931989s for 15>
Nov 12 13:51:10 mmd-VMware-Virtual-Platform systemd-journald[404]: System Journal (/var/log/journal/de98d19e444c43538380c2f1cd33dbaa) is 460.0M, max 1.9G, 1.5G fr>
Nov 12 13:51:16 mmd-VMware-Virtual-Platform systemd-journald[404]: Received client request to flush runtime journal.
Nov 12 13:51:16 mmd-VMware-Virtual-Platform systemd-journald[404]: /var/log/journal/de98d19e444c43538380c2f1cd33dbaa/system.journal: Journal file uses a different>
Nov 12 13:51:16 mmd-VMware-Virtual-Platform systemd-journald[404]: Rotating system journal.
Nov 12 13:51:50 mmd-VMware-Virtual-Platform systemd-journald[404]: /var/log/journal/de98d19e444c43538380c2f1cd33dbaa/user-1000.journal: Journal file uses a differ>
Notice: journal has been rotated since unit was started, output may be incomplete.
lines 1-29/29 (END)
```
and here is the configuration file:
```bash
mmd@mmd-VMware-Virtual-Platform:~$ cat /etc/systemd/journald.conf 
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it under the
#  terms of the GNU Lesser General Public License as published by the Free
#  Software Foundation; either version 2.1 of the License, or (at your option)
#  any later version.
#
# Entries in this file show the compile time defaults. Local configuration
# should be created by either modifying this file (or a copy of it placed in
# /etc/ if the original file is shipped in /usr/), or by creating "drop-ins" in
# the /etc/systemd/journald.conf.d/ directory. The latter is generally
# recommended. Defaults can be restored by simply deleting the main
# configuration file and all drop-ins located in /etc/.
#
# Use 'systemd-analyze cat-config systemd/journald.conf' to display the full config.
#
# See journald.conf(5) for details.

[Journal]
#Storage=auto
#Compress=yes
#Seal=yes
#SplitMode=uid
#SyncIntervalSec=5m
#RateLimitIntervalSec=30s
#RateLimitBurst=10000
#SystemMaxUse=
#SystemKeepFree=
#SystemMaxFileSize=
#SystemMaxFiles=100
#RuntimeMaxUse=
#RuntimeKeepFree=
#RuntimeMaxFileSize=
#RuntimeMaxFiles=100
#MaxRetentionSec=
#MaxFileSec=1month
#ForwardToSyslog=no
#ForwardToKMsg=no
#ForwardToConsole=no
#ForwardToWall=yes
#TTYPath=/dev/console
#MaxLevelStore=debug
#MaxLevelSyslog=debug
#MaxLevelKMsg=notice
#MaxLevelConsole=info
#MaxLevelWall=emerg
#LineMax=48K
#ReadKMsg=yes
#Audit=yes
```

### **querying journal contents**
Switch|Meaning
-|-
-r|show in reverse; newer on top
-f|keep showing the tail
-e|	show and go to the end
-n|	show this many lines from the end (newest ones)
-k|	show kernel message (equal to dmesg)
-b|	show from a specific boot, -1 is the previous one, 0 is the current one. You can check the list using --list-boots
-p|	from specific priority, say -p err
-u|	from a specific systemd unit

>Note:You can also add the name of the program to the command to see the logs from that specific program; say `journalctl /usr/bin/xrdp` or use some fields like `PRIORITY=`, `_PID=` and provide values.

## **systemd-cat**

systemd-cat sends output into the journal (journald). It’s the journald equivalent of `logger`.

**two ways to sue it**
1. Run a command and log its output
    ```bash
    systemd-cat ls /etc
    ```
    Everything the command prints (stdout and stderr) goes into the journal.

2. Pipe text into it
    ```bash
    echo "Backup finished" | systemd-cat
    ```
    With no command, it reads from standard input.


## **managing storage**
e systemd journals can keep their logs in memeory or write them to the disk or drop all the logs. This is determined by the configurations in `/etc/systemd/journald.conf`. The  defoult is:
1. The system checks the `/var/log/journal`. If this directory, exists logs will be saved in a directory inside this. The name of the directory will be decided by looking at `/etc/machine-id`.
2. If the `/var/log/journal` is not there, the logs will be saved in memory at `/run/log/jouranl` and in the directory decided by `/etc/machine-id`.
### The `Storage=` setting
Value	|Meaning
-|-
volatile|	RAM only
persistent|	Disk (creates /var/log/journal if missing)
auto|	Disk only if /var/log/journal already exists (default)
none|	Store nothing

### Limit the size
```
SystemMaxUse=500M       # max disk space for persistent logs
RuntimeMaxUse=100M      # max RAM space for volatile logs
SystemKeepFree=1G       # always leave this much disk free
MaxRetentionSec=1month  # delete entries older than this
MaxFileSec=1week        # how often to start a new journal file
```

------
In case you need to do the clean-up manually (with the help of systemd tools for sure), you can run `journalctl` with these switches:
Switch|	Usage
-|-
--vacuum-time|	clean everything older than this. for example --vacuum-time=3months clean anything older than 3 months. You can use s, m, h for seconds, minutes and days and d/days, months, weeks/w and years/y.
--vacuum-size|	eliminate till logs occupy a specific size; say 1G
--vacuum-files|	Only keep this much archive files

# 108.3 Mail Transfer Agent (MTA) basics 
Softwares those move e-mailes through users. Examples are :Postfix, Exim, Sendmail, and OpenSMTPD.

## **sendmail**
`sendmail` is the oldest MTA. It is hard to config that is not secure enough.

## **postfix**
`postfix` is newer and popular alternative for sendmial.This is default MTA for many disros like ubuntu and RH families.

## sendmail emulation layer
That is the compatibility interface that lets other MTAs and mail tools provide `sendmail`-compatible command, so programs can submit mails the same way no matter which MTA is installed.
Thats why you can type sendmail on whatever distro you are or use the mailq and check your mail regardless of your MTA choice.

## **aliases**
In Mta context `aliases` map a local address to one or more other destinations. They're the traditional way to redirect or fan out mail for local users or system accounts.

The file: `/etc/aliases` (sometimes `/etc/mail/aliases`). Sendmail, Postfix, and Exim all read it. Postfix's location is set by `alias_maps`.

```
# name: destination[, destination...]
postmaster: root
root: alice
webmaster: alice, bob
team: :include:/etc/mail/team.list
backup: /var/mail/archive
logger: |/usr/local/bin/handle-mail.sh
noreply: /dev/null
```

## **sending mail**
We use `mail [receiver]` to send a mail to `receiver` and for check incoming mails we use `mail` comamnd.

```bash
mail mmd
Cc:               #Carbon copy is just secondry reciever we can skip it with just clicking enter
Subject: hi
just saying hi
                  #after we wrote our mail body we use `ctr+d` on a blnak line
mmd@mmd-VMware-Virtual-Platform:~$ mail
"/var/mail/mmd": 1 message 1 new
>N   1 mmd                Tue Nov 24 17:37  13/537   hi
? 1
Return-Path: <mmd@mmd-VMware-Virtual-Platform>
X-Original-To: mmd
Delivered-To: mmd@mmd-VMware-Virtual-Platform.localdomain
Received: by mmd-VMware-Virtual-Platform.localdomain (Postfix, from userid 1000)
	id 2E21843123; Tue, 24 Nov 2026 17:37:37 +0330 (+0330)
To: mmd@mmd-VMware-Virtual-Platform.localdomain
Subject: hi
User-Agent: mail (GNU Mailutils 3.17)
Date: Tue, 24 Nov 2026 17:37:37 +0330
Message-Id: <20261124140737.2E21843123@mmd-VMware-Virtual-Platform.localdomain>
From: mmd <mmd@mmd-VMware-Virtual-Platform>

just saying hi
? q
Saved 1 message in /home/mmd/mbox
Held 0 messages in /var/mail/mmd
mmd@mmd-VMware-Virtual-Platform:~$ mail mmd
Cc: 
Subject: tring to anoying
ha ha ha ha anoyed yoy?
 
mmd@mmd-VMware-Virtual-Platform:~$ mail
"/var/mail/mmd": 1 message 1 new
>N   1 mmd                Tue Nov 24 17:38  13/560   tring to anoying
? 1
Return-Path: <mmd@mmd-VMware-Virtual-Platform>
X-Original-To: mmd
Delivered-To: mmd@mmd-VMware-Virtual-Platform.localdomain
Received: by mmd-VMware-Virtual-Platform.localdomain (Postfix, from userid 1000)
	id 3B11A43123; Tue, 24 Nov 2026 17:38:41 +0330 (+0330)
To: mmd@mmd-VMware-Virtual-Platform.localdomain
Subject: tring to anoying
User-Agent: mail (GNU Mailutils 3.17)
Date: Tue, 24 Nov 2026 17:38:41 +0330
Message-Id: <20261124140841.3B11A43123@mmd-VMware-Virtual-Platform.localdomain>
From: mmd <mmd@mmd-VMware-Virtual-Platform>

ha ha ha ha anoyed yoy?
? d     # we can delet messege using `d` here
? q
Held 0 messages in /var/mail/mmd
```
## **local forwards**
A local forward is a way to automatically redirect your incoming mail to another address, using a file in your home directory called `~/.forward`. You don't need root access.

Simple example: create the file `~/.forward` with one line:
```
you@example.com
```
Now any mail delivered to your local user (for example mmd on this machine) is sent on to `you@example.com` instead of staying in `/var/mail/mmd`.

Keep a local copy too: put a backslash before your username, then the other address:
```
\mmd, you@example.com
```
The `\mmd` means "deliver to my local mailbox, and don't forward again." Without the backslash, it can loop.

>Note: Turn it off => delete the file.

## mailq
`mailq` shows the mail queue: messages your MTA has accepted but not delivered yet. It's the first thing to check when mail seems to be "lost."

# **108.4 Managing printers and printing**

## **CUPS**
CUPS(common unix printing system) is the printing system on linux. It runs a deamon.`cupsd`, that accepts print jobs, queus them, send send them to printers. It uses the  IPP protocol on port 631, and has a web interface at http://localhost:631.

## **Important files and directories**
path|Purpose
-|-
/etc/cups/cupsd.conf	| Main server config (access, listening address, logging)
/etc/cups/printers.conf| Defined printers (queues)
/etc/cups/classes.conf| Printer classes (groups of printers)
/etc/cups/ppd/|	PPD files, one per printer (describe printer features)
/var/spool/cups/|	The print queue (spooled jobs)
/var/log/cups/|	access_log, error_log, page_log

>Note: Don't edit `printers.conf` while cupsd is running, because it gets overwritten. Use the tools or the web interface. After editing `cupsd.conf`, restart: `systemctl restart cups`.

## **Adding and managing printers**
* Web interface: localhost:631 → Administration → Add Printer
* Command line: `lpadmin`
```bash
lpadmin -p office -E -v ipp://192.168.1.50/ipp/print -m everywhere
```
* `-p` printer name
* `-E` enable it and accept jobs
* `-v` device URI
* `-m` driver or model (`everywhere` = driverless IPP)

### Other lpadmin uses:
```bash
lpadmin -d office        # set default printer
lpadmin -x office        # delete printer
```

## **Printing and queue commands (also the "legacy LPD" interface)**
CUPS keeps the old BSD/System V commands for compatibility.
Task|	Command
-|-
Print a file	|lp file.txt or lpr file.txt
Print to a specific printer	|lp -d office file.txt or lpr -P office file.txt
Number of copies	|lp -n 2 file / lpr -#2 file
View queue	|lpq (or lpstat)
Remove a job	|lprm 12 or cancel 12
Remove all your jobs	|lprm -
Cancel all jobs on a printer	|cancel -a office

## **Status and control**
```bash
lpstat -t        # everything: printers, default, queued jobs
lpstat -p        # printers and whether enabled
lpstat -d        # default printer
lpstat -a        # which printers accept jobs
lpstat -v        # device URIs
```
Two separate states matter:
* Enable/disable (`cupsenable` / `cupsdisable`): whether the printer prints queued jobs.
* Accept/reject (`cupsaccept` / `cupsreject`): whether the queue accepts new jobs.

```bash
cupsdisable office     # stop printing, but jobs can still queue
cupsreject office      # refuse new jobs
```
>Note:`lpoptions` sets or shows per-user printer options and defaults (saved in `~/.cups/lpoptions`).

## Quick things to remember
* `lpc` is the old LPD control tool. CUPS only has a limited stand-in, so prefer `cupsenable`/`cupsaccept`.
* `lpr`, `lpq`, `lprm` = BSD style. `lp`, `lpstat`, `cancel` = System V style. CUPS provides both.
* PPD = PostScript Printer Description, stored in `/etc/cups/ppd/`.
* Sharing a printer on the network is done in `cupsd.conf` (`Listen`, `Browsing`, <Location>blocks) or with `cupsctl --share-printers`.
