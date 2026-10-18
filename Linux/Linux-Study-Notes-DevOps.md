# Linux Study Notes for DevOps and Cloud Engineers

Beginner-friendly Linux notes covering Linux fundamentals, directory
structure, commonly used commands, networking, file permissions, users,
services, storage, and basic troubleshooting.

> **Purpose:** Use these notes for Linux learning, DevOps practice,
> cloud support preparation, and interview revision. Commands may differ
> slightly between distributions. Check a command's manual
> (`man command`) before using unfamiliar options.

------------------------------------------------------------------------

## 1. What Is Linux?

Linux is an open-source **operating-system kernel**. The kernel manages
hardware and system resources such as CPU, memory, devices, processes,
and networking.

A **Linux distribution** combines the Linux kernel with system
utilities, package managers, libraries, applications, and sometimes a
graphical desktop to provide a complete operating system.

Examples include Ubuntu, Debian, Fedora, Red Hat Enterprise Linux
(RHEL), Rocky Linux, and Amazon Linux.

### Why Linux is important for DevOps

-   Commonly used on cloud servers and virtual machines.
-   Supports automation through shell scripts.
-   Provides tools for networking, logs, monitoring, and
    troubleshooting.
-   Runs many web servers, databases, containers, and CI/CD tools.
-   Can be managed through a command-line interface (CLI) or graphical
    user interface (GUI).

Linux is Unix-like and follows many traditional Unix design principles,
but Linux is not itself a version of Unix.

------------------------------------------------------------------------

## 2. Linux Filesystem Structure

Linux organizes files and directories in a single directory tree
beginning at `/`, called the **root directory**. The root directory `/`
is different from `/root`, which is the home directory of the root
administrator account.

  -----------------------------------------------------------------------
  Directory                           Purpose
  ----------------------------------- -----------------------------------
  `/`                                 Top of the entire filesystem tree.

  `/bin`                              Essential user commands. On many
                                      modern distributions, it is a
                                      symbolic link to `/usr/bin`.

  `/boot`                             Files needed to start the system,
                                      such as kernels and bootloader
                                      files.

  `/dev`                              Device files representing hardware
                                      and virtual devices.

  `/etc`                              System-wide configuration files.

  `/home`                             Home directories for regular users,
                                      such as `/home/ajay`.

  `/lib`, `/lib64`                    Essential shared libraries and
                                      kernel modules, depending on the
                                      distribution. Often linked into
                                      `/usr` on modern systems.

  `/media`                            Common mount location for removable
                                      media.

  `/mnt`                              Common temporary mount point for
                                      filesystems.

  `/opt`                              Optional or third-party application
                                      software.

  `/proc`                             Virtual filesystem exposing process
                                      and kernel information.

  `/root`                             Home directory for the root
                                      administrator account.

  `/run`                              Runtime data for processes and
                                      services; generally recreated at
                                      boot.

  `/sbin`                             System administration commands.
                                      Often linked into `/usr/sbin` on
                                      modern systems.

  `/srv`                              Data served by system services,
                                      when used by the system
                                      administrator.

  `/sys`                              Virtual filesystem exposing devices
                                      and kernel information.

  `/tmp`                              Temporary files; cleanup behavior
                                      depends on system configuration.

  `/usr`                              Most user-space programs,
                                      libraries, documentation, and
                                      shared resources.

  `/var`                              Variable data such as logs, caches,
                                      queues, and some application data.

  `/var/log`                          Common location for system and
                                      application logs.

  `/var/tmp`                          Temporary files intended to survive
                                      longer than files in `/tmp` on many
                                      systems.
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 3. Basic Linux Commands

### Navigation and files

  ---------------------------------------------------------------------------
  Command                 What it does            Example
  ----------------------- ----------------------- ---------------------------
  `pwd`                   Prints the current      `pwd`
                          directory.              

  `ls`                    Lists directory         `ls -lah`
                          contents.               

  `cd`                    Changes directory.      `cd /var/log`

  `cd ..`                 Moves to the parent     `cd ..`
                          directory.              

  `cd ~`                  Moves to your home      `cd ~`
                          directory.              

  `mkdir`                 Creates a directory.    `mkdir projects`

  `mkdir -p`              Creates parent          `mkdir -p app/logs`
                          directories as needed.  

  `rmdir`                 Removes an empty        `rmdir olddir`
                          directory.              

  `touch`                 Creates an empty file   `touch notes.txt`
                          if it does not exist,   
                          or updates timestamps.  

  `cp`                    Copies a file.          `cp a.txt backup.txt`

  `cp -r`                 Copies a directory      `cp -r app app-backup`
                          recursively.            

  `mv`                    Moves or renames a file `mv old.txt new.txt`
                          or directory.           

  `rm`                    Removes files.          `rm old.txt`

  `rm -r`                 Removes a directory     `rm -r old-folder`
                          tree recursively.       

  `file`                  Identifies a file's     `file archive.tar`
                          type.                   

  `stat`                  Shows file metadata.    `stat notes.txt`

  `ln -s`                 Creates a symbolic      `ln -s /var/log app-logs`
                          link.                   
  ---------------------------------------------------------------------------

**Safety:** `rm -r` and especially `rm -rf` can permanently delete data.
Check the path carefully before running them. Avoid running destructive
commands with `sudo` unless necessary.

### Reading and editing text

  ------------------------------------------------------------------------------------
  Command                 What it does            Example
  ----------------------- ----------------------- ------------------------------------
  `cat`                   Prints a file's         `cat notes.txt`
                          contents.               

  `less`                  Reads a file page by    `less /var/log/syslog`
                          page. Press `q` to      
                          quit.                   

  `head`                  Shows the first 10      `head -n 20 file.txt`
                          lines by default.       

  `tail`                  Shows the last 10 lines `tail -n 50 file.txt`
                          by default.             

  `tail -f`               Follows new lines added `tail -f app.log`
                          to a file.              

  `grep`                  Searches for matching   `grep "ERROR" app.log`
                          text.                   

  `grep -i`               Searches without case   `grep -i "error" app.log`
                          sensitivity.            

  `grep -r`               Searches files in a     `grep -r "TODO" ./app`
                          directory tree.         

  `find`                  Searches for files and  `find /var/log -name "*.log"`
                          directories.            

  `wc`                    Counts lines, words, or `wc -l file.txt`
                          bytes.                  

  `sort`                  Sorts lines.            `sort names.txt`

  `uniq`                  Reports or removes      `sort names.txt \| uniq`
                          adjacent duplicate      
                          lines.                  

  `cut`                   Extracts fields or      `cut -d ',' -f 1 data.csv`
                          character positions.    

  `tr`                    Translates or deletes   `echo hello \| tr 'a-z' 'A-Z'`
                          characters.             

  `diff`                  Compares files line by  `diff old.conf new.conf`
                          line.                   

  `tee`                   Writes command output   `ip address \| tee interfaces.txt`
                          to a file and standard  
                          output.                 

  `sed`                   Processes or transforms `sed 's/old/new/g' file.txt`
                          text streams.           

  `awk`                   Processes structured    `awk '{print $1}' access.log`
                          text by fields and      
                          patterns.               
  ------------------------------------------------------------------------------------

Be careful with `sed -i` because it edits files in place. Test a command
on sample data before applying it to important configuration files.

### System information and monitoring

  -----------------------------------------------------------------------
  Command                 What it does            Example
  ----------------------- ----------------------- -----------------------
  `uname -a`              Displays kernel and     `uname -a`
                          system information.     

  `hostname`              Shows the system        `hostname`
                          hostname.               

  `date`                  Displays date and time. `date`

  `uptime`                Shows how long the      `uptime`
                          system has been running 
                          and load averages.      

  `whoami`                Shows the current       `whoami`
                          username.               

  `id`                    Shows user and group    `id`
                          IDs.                    

  `who`                   Shows logged-in users.  `who`

  `w`                     Shows logged-in users   `w`
                          and their activity.     

  `free -h`               Shows memory usage.     `free -h`

  `df -h`                 Shows filesystem disk   `df -h`
                          space.                  

  `du -sh`                Estimates a directory's `du -sh /var/log`
                          total size.             

  `lsblk`                 Lists block devices and `lsblk`
                          partitions.             

  `dmesg`                 Displays kernel         `dmesg \| tail`
                          ring-buffer messages.   

  `journalctl`            Reads systemd journal   `journalctl -u ssh`
                          logs on systems using   
                          systemd.                

  `history`               Displays shell command  `history`
                          history.                

  `man`                   Opens a command's       `man ls`
                          manual page.            

  `which`                 Finds a command using   `which python3`
                          the current `PATH`      
                          (availability varies).  

  `command -v`            Reports how the shell   `command -v python3`
                          resolves a command.     
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 4. Processes and Performance

A **process** is a running instance of a program. Each process normally
has a process ID (PID).

  -----------------------------------------------------------------------
  Command                 What it does            Example
  ----------------------- ----------------------- -----------------------
  `ps`                    Shows processes.        `ps aux`

  `ps -ef`                Shows processes in      `ps -ef`
                          full-format listing.    

  `top`                   Displays live process   `top`
                          and resource            
                          information. Press `q`  
                          to quit.                

  `htop`                  Interactive process     `htop`
                          viewer, if installed.   

  `pgrep`                 Finds process IDs by    `pgrep nginx`
                          name or pattern.        

  `kill`                  Sends a signal to a     `kill 1234`
                          process ID.             

  `kill -15 PID`          Requests graceful       `kill -15 1234`
                          termination (SIGTERM).  

  `kill -9 PID`           Forces termination      `kill -9 1234`
                          (SIGKILL); use only     
                          when needed.            

  `pkill`                 Sends a signal to       `pkill -TERM appname`
                          processes matching a    
                          pattern.                

  `nice`                  Starts a command with a `nice -n 10 ./job.sh`
                          selected scheduling     
                          priority.               

  `free -h`               Checks memory usage.    `free -h`

  `vmstat`                Reports memory,         `vmstat 1 5`
                          processes, and system   
                          activity.               
  -----------------------------------------------------------------------

Prefer graceful termination before using `kill -9`. Killing the wrong
process can interrupt services or cause data loss.

------------------------------------------------------------------------

## 5. Linux Networking Commands

Modern Linux distributions commonly use the `ip` and `ss` tools. Some
older commands such as `ifconfig`, `netstat`, `route`, and `arp` may
require extra packages and may not be installed by default.

### Connectivity, addresses, and routes

  ---------------------------------------------------------------------------
  Command                  Purpose                 Example
  ------------------------ ----------------------- --------------------------
  `ip address show`        Shows IP addresses on   `ip address show`
                           interfaces.             

  `ip link show`           Shows network           `ip link show`
                           interfaces and their    
                           state.                  

  `ip route show`          Displays the routing    `ip route show`
                           table.                  

  `ip neigh show`          Shows neighbor/ARP      `ip neigh show`
                           information.            

  `ping`                   Tests reachability      `ping -c 4 8.8.8.8`
                           using ICMP, when        
                           permitted.              

  `traceroute`             Shows a possible packet `traceroute example.com`
                           path to a destination;  
                           install if missing.     

  `tracepath`              Traces a path and can   `tracepath example.com`
                           help identify path MTU  
                           issues, if installed.   

  `mtr`                    Combines ongoing        `mtr example.com`
                           reachability tests with 
                           route information, if   
                           installed.              

  `hostname -I`            Prints configured IP    `hostname -I`
                           addresses on many Linux 
                           distributions.          

  `ip route get 8.8.8.8`   Shows the route the     `ip route get 8.8.8.8`
                           kernel would use to     
                           reach an address.       
  ---------------------------------------------------------------------------

### DNS and ports

  ---------------------------------------------------------------------------------------------
  Command                         Purpose                 Example
  ------------------------------- ----------------------- -------------------------------------
  `getent hosts example.com`      Looks up a host using   `getent hosts example.com`
                                  configured name-service 
                                  sources.                

  `nslookup`                      Queries DNS.            `nslookup example.com`

  `dig`                           Queries DNS with        `dig example.com`
                                  detailed output;        
                                  install if missing.     

  `host`                          Performs DNS lookups.   `host example.com`

  `ss -tuln`                      Shows listening TCP/UDP `ss -tuln`
                                  sockets numerically.    

  `ss -tunap`                     Shows TCP/UDP sockets   `sudo ss -tunap`
                                  and process details     
                                  where permissions       
                                  allow.                  

  `curl -I https://example.com`   Requests HTTP response  `curl -I https://example.com`
                                  headers.                

  `curl -v https://example.com`   Shows detailed          `curl -v https://example.com`
                                  connection information. 

  `wget URL`                      Downloads a resource    `wget https://example.com/file.txt`
                                  from a URL.             

  `nc -vz host port`              Tests whether a TCP     `nc -vz example.com 443`
                                  port can be reached, if 
                                  netcat is installed.    

  `tcpdump`                       Captures network        `sudo tcpdump -i any`
                                  packets; often requires 
                                  elevated privileges.    

  `ethtool`                       Displays or changes     `sudo ethtool eth0`
                                  Ethernet device         
                                  settings, if installed. 
  ---------------------------------------------------------------------------------------------

### Remote access and file transfer

  ------------------------------------------------------------------------------------------------
  Command                 Purpose                 Example
  ----------------------- ----------------------- ------------------------------------------------
  `ssh`                   Connects securely to a  `ssh user@server.example.com`
                          remote host.            

  `scp`                   Copies files over SSH.  `scp file.txt user@server:/tmp/`

  `rsync`                 Efficiently             `rsync -av ./site/ user@server:/var/www/site/`
                          synchronizes files and  
                          directories.            

  `curl`                  Transfers data using    `curl -O https://example.com/file.zip`
                          supported protocols.    
  ------------------------------------------------------------------------------------------------

### Older or distribution-dependent networking tools

-   `ifconfig`: legacy interface display/configuration tool; use
    `ip address` and `ip link` on modern systems.
-   `route`: legacy routing-table tool; use `ip route`.
-   `netstat`: legacy connection/statistics tool; use `ss`.
-   `arp`: legacy neighbor-cache tool; use `ip neigh`.
-   `iptables`: firewall management tool still used on some systems, but
    the underlying firewall framework and alternatives vary. Some
    systems use nftables or firewalld.
-   `ifup` / `ifdown`: availability and behavior depend on the network
    configuration system.
-   `telnet`: unencrypted protocol; avoid it for remote administration.
    A port-testing tool such as `nc` is usually more appropriate.

**Important:** Network interface names are not always `eth0`; they may
be names such as `ens5`, `enp0s3`, or others. Find the real interface
name with `ip link show`.

Do not run firewall, route, tunnel, promiscuous-mode, or IP-address
modification commands on a production server unless you understand the
impact. Incorrect changes can disconnect you from the server.

------------------------------------------------------------------------

## 6. File Permissions and Ownership

Linux permissions control what the file owner, group, and other users
can do.

-   `r` = read
-   `w` = write
-   `x` = execute; for directories, execute permission allows
    traversal/access
-   `u` = owner (user)
-   `g` = group
-   `o` = others
-   `a` = all

### View permissions

``` bash
ls -l file.txt
```

Example output:

``` text
-rw-r--r-- 1 ajay developers 120 Oct 18 10:00 file.txt
```

The first character describes the type (`-` for a regular file, `d` for
a directory, `l` for a symbolic link). The next nine characters describe
owner, group, and other permissions.

### `chmod`: change permissions

Symbolic examples:

``` bash
chmod u+rw file.txt
chmod g-x script.sh
chmod o+rx program
chmod u=rw,g=r,o=r file.txt
```

Numeric permissions use these values: read = 4, write = 2, execute = 1.

  Number   Permission
  -------- ------------------------
  `7`      read + write + execute
  `6`      read + write
  `5`      read + execute
  `4`      read only
  `0`      no permissions

Examples:

``` bash
chmod 755 script.sh
chmod 644 file.txt
chmod 700 private-directory
chmod -R u+w project/
```

`chmod 777` grants read, write, and execute permissions to everyone.
Avoid using it as a routine fix because it can create security risks.

### `chown`: change owner and group

``` bash
sudo chown ajay file.txt
sudo chown ajay:developers file.txt
sudo chown -R ajay:developers project/
```

### `chgrp`: change group ownership

``` bash
sudo chgrp developers script.sh
sudo chgrp -R developers project/
```

Use recursive ownership and permission changes carefully: they affect
every matching item under the target directory.

------------------------------------------------------------------------

## 7. Users and Groups

  --------------------------------------------------------------------------------------------
  Command                             Purpose                 Example
  ----------------------------------- ----------------------- --------------------------------
  `whoami`                            Shows the current       `whoami`
                                      username.               

  `id`                                Shows user and group    `id ajay`
                                      IDs.                    

  `sudo command`                      Runs a command with     `sudo systemctl status ssh`
                                      authorized elevated     
                                      privileges.             

  `su - username`                     Switches user and loads `su - ajay`
                                      that user's login       
                                      environment.            

  `passwd`                            Changes a password.     `passwd`

  `sudo useradd username`             Creates a user; options `sudo useradd ajay`
                                      and defaults vary by    
                                      distribution.           

  `sudo adduser username`             Interactive             `sudo adduser ajay`
                                      user-creation helper on 
                                      Debian/Ubuntu.          

  `sudo usermod -aG group username`   Adds a user to a        `sudo usermod -aG docker ajay`
                                      supplementary group.    

  `sudo groupadd groupname`           Creates a group.        `sudo groupadd developers`

  `groups username`                   Shows group             `groups ajay`
                                      memberships.            

  `sudo userdel username`             Deletes a user account. `sudo userdel ajay`
  --------------------------------------------------------------------------------------------

Use `usermod -aG` to append a supplementary group. Omitting `-a` can
replace existing supplementary group memberships. After group changes,
the user may need to start a new login session.

------------------------------------------------------------------------

## 8. Packages, Services, and Logs

Package-management commands depend on the Linux distribution.

### Package management

Ubuntu and Debian:

``` bash
sudo apt update
sudo apt install nginx
sudo apt remove nginx
```

Fedora and many RHEL-family systems:

``` bash
sudo dnf install nginx
sudo dnf remove nginx
```

Older RHEL-family systems may use `yum`; on many modern systems, `yum`
is compatible with or linked to `dnf`.

### Services with `systemctl`

On systems using systemd:

``` bash
sudo systemctl status nginx
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
sudo systemctl enable nginx
sudo systemctl disable nginx
```

-   `start` starts a service now.
-   `stop` stops it now.
-   `restart` stops and starts it again.
-   `reload` asks a service to reload its configuration if supported.
-   `enable` configures it to start at boot.
-   `status` shows its current state and recent messages.

Service names differ by distribution. For example, the SSH service is
often named `ssh` on Ubuntu and `sshd` on some other distributions.

### Logs

``` bash
journalctl
journalctl -u nginx
journalctl -u nginx --since "1 hour ago"
journalctl -f
```

Log file locations vary by distribution and service. On Ubuntu, for
example, general system logs may be in `/var/log/syslog`, while some
RHEL-family systems use `/var/log/messages`. Use `journalctl` where
systemd journal logs are available.

------------------------------------------------------------------------

## 9. Disk, Archives, and Mounts

  ------------------------------------------------------------------------------------
  Command                 Purpose                 Example
  ----------------------- ----------------------- ------------------------------------
  `df -h`                 Shows filesystem        `df -h`
                          free/used space.        

  `du -sh path`           Estimates total size of `du -sh /var/log`
                          a path.                 

  `lsblk`                 Lists block devices.    `lsblk`

  `blkid`                 Shows filesystem UUIDs  `sudo blkid`
                          and types; may require  
                          privileges.             

  `mount`                 Shows mounted           `mount`
                          filesystems or mounts a 
                          filesystem.             

  `umount`                Unmounts a filesystem.  `sudo umount /mnt/data`

  `tar -cvf`              Creates a tar archive.  `tar -cvf backup.tar project/`

  `tar -czvf`             Creates a               `tar -czvf backup.tar.gz project/`
                          gzip-compressed tar     
                          archive.                

  `tar -xzvf`             Extracts a              `tar -xzvf backup.tar.gz`
                          gzip-compressed tar     
                          archive.                

  `gzip`                  Compresses a file.      `gzip file.txt`

  `gunzip`                Decompresses a gzip     `gunzip file.txt.gz`
                          file.                   

  `zip -r`                Creates a ZIP archive;  `zip -r site.zip site/`
                          requires zip package.   

  `unzip`                 Extracts a ZIP archive. `unzip site.zip`
  ------------------------------------------------------------------------------------

Do not unmount a filesystem that is in active use. Before changing
partitions, filesystems, or mounts on a server, verify the device and
ensure backups exist.

------------------------------------------------------------------------

## 10. Shell, Environment Variables, and Job Scheduling

  -----------------------------------------------------------------------
  Command                 Purpose                 Example
  ----------------------- ----------------------- -----------------------
  `echo`                  Prints text or variable `echo "Hello Linux"`
                          values.                 

  `export`                Exports an environment  `export APP_ENV=dev`
                          variable to child       
                          processes.              

  `env`                   Prints environment      `env`
                          variables.              

  `alias`                 Defines a shell         `alias ll='ls -lah'`
                          shortcut.               

  `source file.sh`        Runs a shell file in    `source ~/.bashrc`
                          the current shell.      

  `sleep`                 Waits for a specified   `sleep 5`
                          number of seconds.      

  `jobs`                  Lists jobs in the       `jobs`
                          current shell.          

  `bg`                    Resumes a stopped job   `bg %1`
                          in the background.      

  `fg`                    Brings a shell job to   `fg %1`
                          the foreground.         

  `crontab -e`            Edits the current       `crontab -e`
                          user's cron schedule.   

  `crontab -l`            Lists the current       `crontab -l`
                          user's cron jobs.       

  `shutdown`              Shuts down or schedules `sudo shutdown now`
                          shutdown, depending on  
                          options.                

  `reboot`                Reboots the system.     `sudo reboot`
  -----------------------------------------------------------------------

### Cron example

Run a script every day at 2:30 AM:

``` cron
30 2 * * * /home/ajay/backup.sh
```

Cron uses the system's configured time zone and environment. Use
absolute paths and redirect output to a log when appropriate. Test
scripts manually before scheduling them.

------------------------------------------------------------------------

## 11. Top 100 Linux Commands --- Quick Revision

This is a list of 100 distinct commands or common command utilities.
Some are optional packages and may not be installed by default.

1.  `pwd` --- show current directory
2.  `ls` --- list files
3.  `cd` --- change directory
4.  `mkdir` --- create directory
5.  `rmdir` --- remove an empty directory
6.  `touch` --- create a file/update timestamp
7.  `cp` --- copy files
8.  `mv` --- move or rename files
9.  `rm` --- remove files
10. `cat` --- display file contents
11. `less` --- read files page by page
12. `head` --- show the beginning of a file
13. `tail` --- show the end of a file
14. `grep` --- search text
15. `find` --- find files and directories
16. `locate` --- search an indexed file database, if installed
17. `file` --- identify file type
18. `stat` --- show file metadata
19. `chmod` --- change permissions
20. `chown` --- change owner/group
21. `chgrp` --- change group ownership
22. `umask` --- view/set default permission mask
23. `ln` --- create links
24. `readlink` --- inspect symbolic links
25. `echo` --- print text
26. `printf` --- formatted output
27. `tee` --- write output to a file and terminal
28. `wc` --- count lines/words/bytes
29. `sort` --- sort lines
30. `uniq` --- handle adjacent duplicate lines
31. `cut` --- extract fields
32. `tr` --- translate/delete characters
33. `sed` --- transform text
34. `awk` --- process text by fields/patterns
35. `xargs` --- build command arguments from input
36. `diff` --- compare files
37. `patch` --- apply a patch
38. `tar` --- archive files
39. `gzip` --- compress files
40. `gunzip` --- decompress gzip files
41. `zip` --- create ZIP archives, if installed
42. `unzip` --- extract ZIP archives
43. `bzip2` --- bzip2 compression, if installed
44. `xz` --- xz compression, if installed
45. `df` --- report filesystem space
46. `du` --- estimate file/directory size
47. `lsblk` --- list block devices
48. `blkid` --- show block-device attributes
49. `mount` --- mount/list filesystems
50. `umount` --- unmount filesystems
51. `free` --- show memory usage
52. `uptime` --- show uptime/load average
53. `uname` --- show system/kernel information
54. `hostname` --- show system hostname
55. `date` --- show date/time
56. `cal` --- display a calendar, if installed
57. `whoami` --- show current username
58. `id` --- show user/group IDs
59. `who` --- show logged-in users
60. `w` --- show logged-in users and activity
61. `last` --- show login history from available records
62. `ps` --- list processes
63. `top` --- live process monitor
64. `htop` --- interactive process monitor, if installed
65. `pgrep` --- find process IDs by pattern
66. `pkill` --- signal processes by pattern
67. `kill` --- signal a process by PID
68. `killall` --- signal processes by name; behavior varies
69. `nice` --- start a process with a selected priority
70. `renice` --- change a process priority
71. `vmstat` --- report system activity
72. `iostat` --- report CPU/I/O statistics, if installed
73. `lsof` --- list open files, if installed
74. `strace` --- trace system calls, if installed
75. `systemctl` --- manage systemd services
76. `journalctl` --- query systemd logs
77. `dmesg` --- show kernel messages
78. `service` --- service helper on some systems
79. `crontab` --- manage cron schedules
80. `at` --- schedule a one-time job, if installed/configured
81. `sudo` --- run authorized elevated commands
82. `su` --- switch user
83. `passwd` --- manage passwords
84. `useradd` --- create user
85. `usermod` --- modify user
86. `userdel` --- delete user
87. `groupadd` --- create group
88. `groups` --- show group membership
89. `getent` --- query system databases
90. `ip` --- manage/view networking
91. `ping` --- test IP reachability
92. `ss` --- inspect sockets
93. `curl` --- transfer data to/from URLs
94. `wget` --- download resources
95. `ssh` --- secure remote login
96. `scp` --- copy files over SSH
97. `rsync` --- synchronize files
98. `dig` --- query DNS, if installed
99. `tcpdump` --- capture network packets, if installed
100. `man` --- read command manuals

------------------------------------------------------------------------

## 12. Useful DevOps Troubleshooting Examples

### Check whether a service is running

``` bash
sudo systemctl status nginx
```

### Check whether a port is listening

``` bash
sudo ss -tuln
```

### Find recent errors in a log

``` bash
grep -i "error" /var/log/syslog
```

The file path varies by distribution and service. For systemd-managed
services, you can also use:

``` bash
sudo journalctl -u nginx --since "30 minutes ago"
```

### Check disk space and large log directories

``` bash
df -h
sudo du -sh /var/log
```

### Check memory and CPU/process activity

``` bash
free -h
top
```

### Check DNS and HTTPS connectivity

``` bash
getent hosts example.com
curl -I https://example.com
```

### Find a file by name

``` bash
find /var/log -type f -name "*.log"
```

------------------------------------------------------------------------

## 13. Best Practices for Beginners

1.  Understand a command before running it with `sudo`.
2.  Check the current directory with `pwd` before deleting or changing
    files.
3.  Use `man command` or `command --help` to learn options.
4.  Prefer `ip` and `ss` over older networking tools when available.
5.  Avoid `chmod 777` as a default solution.
6.  Do not expose passwords, private keys, tokens, or secrets in shell
    history or GitHub.
7.  Back up configuration files before editing them.
8.  Use absolute paths in scripts and scheduled jobs.
9.  Check service logs and exit codes when troubleshooting.
10. Practice risky commands in a disposable virtual machine, not a
    production server.

------------------------------------------------------------------------

## 14. Practice Tasks

-   [ ] Create a directory named `linux-practice` and create three files
    inside it.
-   [ ] Copy one file, rename another, and remove a test file.
-   [ ] Use `grep`, `head`, `tail`, `wc`, and `find` on sample files.
-   [ ] Create a script and set its permissions using `chmod 755`.
-   [ ] Create a test user and group in a lab machine.
-   [ ] Check IP addresses, routes, DNS resolution, and listening ports.
-   [ ] Check disk space, memory usage, and running processes.
-   [ ] Inspect a service using `systemctl` and its logs using
    `journalctl`.
-   [ ] Create and extract a `.tar.gz` archive.
-   [ ] Schedule a harmless test script using cron, then verify its
    output.

------------------------------------------------------------------------

## 15. Final Summary

Linux is a foundational skill for DevOps and cloud roles. Learn
filesystem navigation, file operations, permissions, users, processes,
services, logs, storage, networking, and shell scripting. Regular
hands-on practice is more useful than memorizing commands alone.

**Suggested repository filename:** `README.md` or `Linux-Study-Notes.md`

**Suggested GitHub repository description:** Beginner-friendly Linux
commands, filesystem structure, networking, permissions, system
administration, and DevOps troubleshooting notes.
