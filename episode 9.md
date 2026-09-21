# **104.1 Create partitions and file systems**
## **Block device**
A block device is an unorddered sstorage(user can accesse to data in any order) and nonvolatile (data will save even after shut the power) like hard disks, ssd, usb memory, flopy and... . 

We can see list of block devices using `lsblk` command and `ls -l /dev/ | grep ^b` (In addition on a long `ls` format (`-l`), block devices are shown with a `b at the first column) 
>Note: In some desttroes of linux we have `loop` blockes that is a way to treat a regular file as if it were a block device (like a disk or partition). Normally, block devices are physical things: /dev/sda, /dev/sdb1, etc. A `loop` device (`/dev/loop0`, `/dev/loop1`, ...) lets the kernel "attach" a file to one of these virtual device slots, so tools that expect a disk can operate on that file instead. **Ubuntu's snap packages actually use loop devices under the hood — every installed snap is mounted as a squashfs image via a loop device**.

It is possible to create partitions on a block device and even split it and use it as multiple disks.
>Linux systems use `udev` to add block devices and their paritions to the `/dev` in the form of ``/dev/sdb1`` (2nd disk (`b`) and first parition (`1`)).

## **Editing Partition Tables**
### **fdisk**
`fdisk` iss main tool for for viewing / changing and creating partitions on MBR systems. the `-l` switch lists the partitions.

> Note: It is also possible to run fdisk in intractive mode. pass the path of device as arguman to fdisk.

### **gdisk**
We use `gdisk` on GPT machines. Its not that different from `fdisk`.

### **parted**
`parted` is a `GNU` tool to edit partitions. It's main advantage is the ability of resize currently defined partitions.
>Note: `gparted` is geraphical tool(graphical sibling of prted).

## **Filesystems and Formating the partitions**

### **Filesystems**
After partitioning block device it must be formated to be usable to store files and directories.Formatting a file system, creates a map which stores the location and name of files and directories and make it possible to move files between folders, deleting them or renaming them; think of it as the index of a book.

### common used filesystem in linux:
format|description
------|-----------
ext2|econd extended filesystem was developed to address shortcomings in the older Unix/Minix filesystem used in early versions of Linux. It has been used extensively on Linux for many years. There is no journaling in ext2, and it has largely been replaced by ext3 and more recently ext4.
ext3|ext2 + journaling, max file size is 2TB and max filesystem size is 16TB
ext4|urrent version of ext, max file size is 16TB and max filesystem size is 1EB (1000*1000TB)
XFS|journaling, caches to RAM, great for uninterruptible power supplies, Max file and filesystem size is 8EB
swap|Swap is used when the system needs to use more ram than what is has. It's like an extra ram on disk
VFAT|FAT32, no journaling, good for data exchange with windows, does not understand permissions and symbolic links
exFAT|Extended FAT. A newer version of FAT which is used mainly for extended device which should work on all machines; like USB disks
bttrfs|A new high performance file system. Max file and filesize is 16 EB. Has its own form of RAID and LVM and build-in snapshots and fault tolerance and data compression on the fly.

### **Creating filesystems**
You can format your partitions with `mkfs` command (and `mkswap` for swap). This is a front end to commands like `mkfs.ext3` for ext3, `mkfs.ext4` for ext4 and `mkfs.reiserfs` for ReiserFS. full list of installed on your system is `ls /sbin/mk*`
`mkfs` hs to common switch: `-t` for specify the format and If you need to assign a lable to the partition, you have to use the `-L lable_name` option.

>Note:  recent system, people use UUIDs instead of labels. UUID is general name of partition and it is independent to system and os.


# **104.2 Maintain the integrity of filesystems**
## **df &du**

In many cases you want to find out about the free space of a disk or find how much space a directory is using or check how many inodes left.
### **df**
The diskfree command is used to find out about the free and used space on file systems.
 >Note: On some filesystems (like ext2-4) we have a fixed number of inodes, so you may need to check the number of remaining inodes too. To do so, use the `-i` switch.

### **du**
 
The diskusage command shows the used space of directories and files.
### common switches:
switch | usage
------|-------
-h|print sizes in powers of 1024
-H|print sizes in powers of 1000
-c|show the grand 
total--max-depth N|Calculate all but show only N directories deep
-s|Only shows the summary and not all the directories one by one

## **Checking filesystems**

### **fsck**
It's a command for check and repair filesystems.things like corrupted inodes, orphaned blocks, bad directory entries, or an incorrectly marked "dirty" filesystem after an unclean shutdown (power loss, crash, etc.).
 ># 104.2 is very boring i think it's unnecessary so i'll skip it.
              
# **104.3 control mounting and unmounting of iflesystems**

## **mounting and unmounting**
A formated partition needs to be somewhere under `/` directory  so os can see it. We call it mounting. When we mount a partition in a directory it acts like a virtual subdirectories somewhere in your `/` tree but actuly it is separated partition.
for mounting we use `mount` command.

**syntax**:
```bash
mount [options] <device> <mount point>
```
>Note: read more about `mount` options.

To unmount, simply use the `umount` on the drive or the directory (These two are equivalent).

## **UUID & Labels**
A problem is lable of a partition or derive can change in differrent systems or even after remove and reconnect. For example `dev/sdc` is lable of one of my block devices and after i reconnect it or move it to a different system it could  be `dev/sdl` or etc. So we solve it with `UUID`. It's a gelobale id for my block device.

## **fstab**
For automatic mounting, Linux uses the /etc/fstab file. Its like a table which shows what file system should be mounted where during the boot.

# **104.5 Manage file permissions and ownership**

## **Users and Groups**

A linux system can have many users and groups. Each user can be in many groups(have a primary group) and a group can have many users.`whoami` command shows your user and `groups` command shows group memberships for each USERNAME or, if no USERNAME is specified, for the current process (which may differ if the groups database has changed). `id` shows both user and group information.
>Note:Note the 0 UID and 0 GID belong to the root user and root group.

>Note:These are stored in /etc/passwd and /etc/group files.

## **File ownership & permissions**
In linux we have three layers of permision: user, group, other.
ach file belongs to one user and one group and this user and the members of that group will have specific read/write/execute accesses on it.

```bash
mmd@mmd-VMware-Virtual-Platform:~$ ls -ltrh tree.txt output
-rw-rw-r-- 1 mmd  mmd   42M Aug  1 16:41 tree.txt
-rw-r--r-- 1 root root 4.4K Sep 14 20:07 output
```

As you can see `tree.txt` blongs to aq user named `mmd` and a group named `mmd` (first `mmd` is user and second is group). Also you can see `output` belongs to `root`.
Another thing we can see is permisions/access;. As you can see `-rw-rw-r--`.

position|Meaning
---|--
1|What this entry is. Dash (-) is for ordinary files, 'l' is for links & 'd' is for directory
2,3,4|read, write and execute access for the owner
5,6,7|read, write and execute access for the group members
8,9,10|read, write and execute access for other users

## **Changing permissions**
`chmod` is a tool for changing permison of files or directory.You can use octal codes or short codes.
When using octal codes, you have to to create an octal number to tell chmod what you want to do. In this method, 0 means no access, 1 means execute, 2 means write and 4 means read. So if you want to give read+execute, you have to give 4+1 which is 5.

symbolic|Octal
-|-
rwx|7
rw-|6
r-x|5
r--|4
-wx|3
-w-|2
--x|1
---|0

### example:
So if you want to give rwx to the owner, rx to the group and only x to others, you have to use 751:
```bash
chmode 751 tree.txt
```
> Note: One very common switch on chmod is -R for recursive chmoding on files.

## **Changing owner and groups**
`chown` can change the owner of a file or directory and `chgrp` can change the group.

> Note: It is possible to assign more groups to a user via usermod command

## **Access modes**
Normally when you run a program, it runs with your access level. But what happens if you need to run a program with a higher access level? say to do changes in the password files? For this Linux sets two special bits for each file; suid (set user id) and sgid (set group id). If these bits are set on a file, that file will be executed with the access of the owner (or group) of the file and not the user who is running it.

``` bash
$ ls -ltrh /usr/bin/passwd
-rwsr-xr-x 1 root root 50K Jul 18  2014 /usr/bin/passwd
```
`s` in the executable bit for the user permissions means it will be executed with the access of the owner.

It is possible to set/unset the suid and sgid using `chmod` and `+s` or `-s` instead of `x`.

The last special bit is called stiky bit. If set, makes the file resistant to deletion.

access mode|octal|symbolic
---|-|-
suid|4000|u+s
guid|2000|g+s
sicky|1000|+t

## **umask**
`umask` controls the default permissions given to newly created files and directories. It doesn't set permissions directly — it subtracts (masks out) permission bits from a maximum starting point.

# **104.6 Creat and change hard and symbolic link**

## **links**
A link is simply an additional directory entry for a file or directory.Every file on a Linux filesystem has an inode — a data structure holding metadata (permissions, owner, size, timestamps, and pointers to the actual data blocks on disk). The filename is just a label that points to an inode.

### **hard link**
A hard link is a second filename pointing to the same inode. There's no "original" and "copy" both names are equally valid references to the same data.

### **soft link**
A soft link is a separate file that contains a path to another file. Think of it like a shortcut.

>Note: If original is deleted in hard link Data persists (link count) but in soft link Becomes broken/dangling 


# **104.7 Find system files and place files in the cottect location**

## **FHS**
Filesystem Hierarchy Standard (FHS) is a document describing the Linux/Unix file hierarchy. It is very useful to know these because it lets you easily find what you are looking for as a system admin.

|direcory|usage|
|--|-
/|Primary hierarchy root and root directory of the entire file system hierarchy
/bin| Essential command binaries
/boot | Static files of the boot loader
/dev | Device files
/etc | Host-specific system configuration
/lib | Essential shared libraries and kernel modules
/media | Mount point for removable media
/mnt | Mount point for mounting a filesystem temporarily
/opt | Add-on application software packages
/sbin | Essential system binaries
/srv | Data for services provided by this system
/tmp | Temporary files
/usr | Secondary hierarchy
/var | Variable data
/home | User home directories (optional)
/lib | Alternate format essential shared libraries (optional)
/root | Home directory for the root user (optional)

### sample 
```
/
├── bin/          → Essential user command binaries (ls, cp, cat, bash) — usable in single-user mode
├── boot/         → Boot loader files: kernel (vmlinuz), initramfs, GRUB config
├── dev/          → Device files (e.g. /dev/sda, /dev/null, /dev/tty)
├── etc/          → System-wide configuration files (static, host-specific)
│   ├── passwd
│   ├── fstab
│   └── ssh/
├── home/         → Users' personal directories
│   └── mohamed/
├── lib/          → Essential shared libraries for /bin and /sbin binaries
├── media/        → Mount points for removable media (USB drives, CDs)
├── mnt/          → Temporary mount point for manually mounted filesystems
├── opt/          → Optional/third-party software packages (self-contained)
├── proc/         → Virtual filesystem — kernel & process info (not real files on disk)
├── root/         → Home directory for the root user (NOT /root of the filesystem — this is root's home)
├── run/          → Runtime variable data since boot (PIDs, sockets) — replaces old /var/run
├── sbin/         → Essential system binaries for administration (fdisk, reboot, iptables)
├── srv/          → Data for services provided by this system (web/ftp server data)
├── tmp/          → Temporary files, cleared on reboot (sticky bit set, see earlier)
├── usr/          → Secondary hierarchy — user applications & data (read-only, shareable)
│   ├── bin/      → Non-essential user binaries
│   ├── lib/      → Libraries for /usr/bin, /usr/sbin
│   ├── local/    → Locally compiled/installed software (not from package manager)
│   ├── sbin/     → Non-essential system binaries
│   └── share/    → Architecture-independent data (docs, man pages, icons)
└── var/          → Variable data — files that change during normal operation
    ├── log/      → Log files (syslog, auth.log, etc.)
    ├── spool/    → Spooled data (mail, print queues, cron jobs)
    ├── cache/    → Application cache data
    └── tmp/      → Temporary files preserved between reboots (unlike /tmp)
```


## **PATH**
A general linux install has a lot of files. So how the shell finds and runs a command? This is done by a variable called PATH.
this is the list of directories separated with colon. Obviously you can change your path with `export PATH=$PATH:/usr/new/dir `or put this in `.bashrc` to make it permanent.

## **locating file**
### **which, type, whereis**
The `which` command shows the first appearance of the command given in the PATH. In other words the `which mkfs` will tell you what will be run if you issue this `mkfs` command.
>Note: use the `-a` switch to show all appearance in the path and not only the first one.
### example
```bash
mmd@mmd-VMware-Virtual-Platform:~$ which mkfs
/usr/sbin/mkfs
mmd@mmd-VMware-Virtual-Platform:~$ which -a mkfs
/usr/sbin/mkfs
/sbin/mkfs
```
The `type` shows you what is that.
### example
```bash
mmd@mmd-VMware-Virtual-Platform:~$ type cd
cd is a shell builtin
mmd@mmd-VMware-Virtual-Platform:~$ type mkfs
mkfs is /usr/sbin/mkfs
```

Another useful command in this category is `whereis`. Unlike `which`, `whereis` shows `man` pages and source codes of programs alongside their binary location.

## **find**
`find` searches for files and directories based on criteria — name, type, size, time, permissions, owner — and can act on the results. Unlike `locate` (which searches a pre-built database), `find` searches the filesystem live, so it's always current but slower.
```bash
find [path] [expression]
```
### example
```bash
find /home -name "file.txt"        # exact match, case-sensitive
find /home -not -name "*.log"      # everything except .log files
find /home -iname "file.txt"       # case-insensitive
find /var -type f        # regular files only
find /var -type d        # directories only
find /var -type l        # symbolic links only
find / -size +100M       # larger than 100 MB
find / -size -1k         # smaller than 1 KB
find / -size 50c         # exactly 50 bytes
```

## **locate & updatedb**
there is an issue with `find`,it does a live active search! This can slow down your system or push too much pressure on your hard or on larger file systems take too long.To solve this we use `locate`.`locate` finds files by name instantly by searching a pre-built database instead of scanning the live filesystem (the opposite tradeoff from `find`).

### example
```bash
locate filename.txt
locate "*.conf"              # wildcard search
locate -i readme             # case-insensitive
```
This is fast because its data comes from a database created with `updatedb` (its package is called `plocate` on debian). Usually this command runs automatically (with a cronjob) on a daily basis. Its configuration file is `/etc/updatedb.conf` or `/etc/sysconfig/locate`.

>Note: You can update the db by running `updatedb` as root.