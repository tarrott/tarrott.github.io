# Linux
- command docs: `man find`
- run previous command with sudo: `sudo !!`


---
## Package Management
- [comparison](https://wiki.archlinux.org/index.php/Pacman/Rosetta)
#### apk
#### apt
- view installed packages: `apt list --installed`
- specific packge: `apt list -a pkgNameHere`
- updates
    - `apt update`
    - `apt list --upgradable`
    - `apt upgrade`
    - update dev version (e.g. 18.04.3 to 18.04.5): `apt dist-upgrade` & `reboot`
    - update specific package:
        - e.g. apache2: `add-apt-repository ppa:ondrej/apache2 && apt update && apt install apache2`
- remove package files no longer needed: `apt autoclean`
#### dpkg
#### yum
    - `yum update`
#### pacman

---
## Compile from Source
#### Debian/Ubuntu
- install essential build tools: `sudo apt install build-essential`
###### nmap example
- check current nmap version: `nmap -V`
- get tarball link for the latest development release for nmap from nmap.org download section under "source code distribution"
- `wget -v https://nmap.org/dist/nmap-7.91.tar.bz2`
- uncompress the bzip2 file with: `tar -jxvf nmap-7.91.tar.bz2`
    - `-j` flag filters the archive through bzip2
    - `-x` flag for extract
    - `-v` flag for verbose
    - `-f` flag specifies the following filename
- read the INSTALL file to get compile instructions:
    - `./configure`
    - `make`
    - `sudo make install` (uses root privileges because it installs files outside of home directory)
- update the index of files the `locate` command uses: `sudo updatedb`
- find nmap installs: `locate /bin/nmap`
    - In general, /bin is for key parts of the operating system, /usr/bin for less critical utilities and /usr/local/bin for software chosen to be installed manually. When a command is run it will search through each of the directories given in the PATH environment variable, and use the first match. So, if /bin/nmap exists, it will run instead of /usr/local/bin
- Security Risk NOTE: since namp was installed outside of the apt system, this binary will not get updates when `apt update` is ran. This is okay for a utility like namp, but for an exposed service (apache, mysql, etc.) it is important to track security alerts for the application (including its dependencies), and patch it to the latest versions when they're available. This is both tedius and a serious security risk if not done.

---
## Configuration
- `hostnamectl set-hostname <name>`
    - the line `preserve_hostname` might need to be changed to `true` in `/etc/cloud/cloud.cfg` to save hostname for cloud servers after server reboot
    - old way hostname: edit `/etc/hostname` & rename `/etc/host` entries
- Disable hibernation: `sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target`
- No password configured: `sudo passwd`
- `hostname -f`
- `dpkg-reconfigure tzdata / timedatectl set-timezone`
- fdisk
- cmd prompt `export PS1="\[\033[36m\]\u\[\033[m\]@\[\033[32m\]\h:\[\033[33;1m\]\w\[\033[m\]\$ "`

#### SSH Server Config
- edit: `/etc/ssh/sshd_config`
    - disable root login: `PermitRootLogin no`
    - enable key checking: `PubkeyAuthentication yes`
    - disable password logins:
        - `PasswordAuthentication no`
        - `PasswordAuthentication no`
        - `UsePAM no`


#### SSH Client Config
`~/.ssh/config`
```
host shortcutname
HostName server.domain.com
Port 5555
User username
```

---
## User Administration
- add user: `adduser <user>`
- add group: `addgroup <group>`
- asign group: `adduser <user> <group>`
- delete user: `deluser <user>`
- `useradd`, `usermod`, and `userdel` are low level utlities and the above methods should usually be used by sysadmins
- assign group: `usermod -aG <group> <username>`
- give sudo: 
    - Deb: `usermod -aG sudo <user>`
    - Fed: `usermod -aG wheel <user>`
- copy ssh key: `ssh-copy-id -i $HOME/.ssh/id_rsa.pub <user@ip>`
- give permissions to run specific commands: `visudo`
```
# Allow user "helper" to run "sudo reboot"
# ...and don't prompt for a password
#
helper ALL = NOPASSWD:/sbin/reboot
```

---
## Cloud VMs

#### GCP
setup root pass: `sudo passwd`

---
## System Diagnostics
- `uptime -p`
- `uptime` also shows load average and number of users currently logged in

"The three numbers show the load averages for the last minute, 5 minutes, and 15 minutes respectively. A load average of 1 reflects the full workload of a single processor on the system."

#### CPU
- number of processors: `grep processor /proc/cpuinfo | wc -l`

#### Memory
- `free -h`
- `cat /proc/meminfo`
- `vmstat -s`
    - `vmstat 5 10` displays a system’s virtual memory statistics 10 times at 5-second intervals.
        - `so` shows the amount of data being moved to the swap to free up memory
        - `si` shows the amount of data being pulled from the swap back into memory
    - When a server is constantly swapping into and out of memory, that indicates that the load on it is too great for the available resources.
    - If your system consumes most of its swap area, that might mean that the server is trying to do more than its available memory permits not that it’s low on memory.
- `top` and `htop`
    - sort by CPU (P) or Memory (M)

#### Disk
- `df -h`

#### Processes
- `ps -ef`
- `pgrep` and `pidof`
- services (process name): `service --status-all`
    - if services is unavailable check `/usr/sbin/service`
    - add to path

---
## Finding files
- locate file anywhere in the filesystem: `locate access.log`
    - requires updated db: `sudo updatedb`
- executable location: `which vim`
- location of file within a specific dir containing name: `find /var -name access.log`
- files that have been edited in the last 3 hours: `find /home -mtime -3`
- search through a directory of text files: `grep -R -i "PermitRootLogin" /etc/*`


## Log Rotation
- `logrotate` usually automatically configured and ran by cron in `/etc/cron.daily/logrotate`
- overall logrotate config file stored in `/etc/logrotate.conf`
- individual logrotate recipes are stored in `/etc/logrotate.d/`
- for example, the `/etc/logratote.d/apache2` config file could be edited so that the log file is emailed to an auditor each time it is rotated


---
## File System Management
- check disk usage: `ncdu`
- cp -R
- mkdir -p
- touch
- mv
- rm
- scp
- tree
- set the sticky bit for a directory: `chmod 1777 <dir_name>`
    - only the owner of each file in this directory can delete the file

#### Links
###### Hard links
- `ln /path/to/target/file hardlinkname`
- can only link to files, not directories
- cannot link to remote files on different partitions/disks
- links reference the same inode as the target file, and therefore the physical location on disk
    - if the target file is moved or deleted, the hard link will still retain a copy of the original contents

###### Symbolic links
- `ln -s /path/to/target softlinkname`
- can additionally link to directories, remote files, and files on different partitions/disks
- links will no longer reference the target file if it has been moved or deleted
- symlinks get their own inode because they are referencing abstract filenames/dirs and not the target's physical location
    - if the target is recreated with the same name then the symlink will reference this new target


#### ACLS
- allow for more details filesystem permissions (e.g. allowing a user who is not the owner or in the file's group to access the file)
- `getfacl`
- `setfacl`

#### Filepaths
- `man hier` for filesystem hierarchy
- [Filesystem Hierarchy Standard](http://www.pathname.com/fhs/)
- [Linux Filesystem Tree Overview](https://help.ubuntu.com/community/LinuxFilesystemTreeOverview)

###### Daily Driver
- repos: `~/repos/github/`
- `/home` home sweet home, this is the place for users' home directories.

###### Application Development
- `/opt` stores additional software not handled by the package manager.
- `/tmp` is a place for temporary files used by applications
- `/var `is dedicated to variable data, such as logs, databases, websites, and temporary spool (e-mail etc.) files that persist from one boot to the next. A notable directory it contains is /var/log where system log files are kept.

###### System
- `/bin` is a place for most commonly used terminal commands, like ls, mount, rm, etc.
- `/etc` contains system-global configuration files, which affect the system's behavior for all users.
- `/root` is the superuser's home directory, not in /home/ to allow for booting the system even if /home/ is not available.
- `/usr` contains the majority of user utilities and applications, and partly replicates the root directory structure, containing for instance, among others, /usr/bin/ and /usr/lib.
- `/boot` contains files needed to start up the system, including the Linux kernel, a RAM disk image and bootloader configuration files.
- `/dev` contains all device files, which are not regular files but instead refer to various hardware devices on the system, including hard drives.
- `/lib` contains very important dynamic libraries and kernel modules
- `/media` is intended as a mount point for external devices, such as hard drives or removable media (floppies, CDs, DVDs).
- `/mnt` is also a place for mount points, but dedicated specifically to "temporarily mounted" devices, such as network filesyste


## Reading Files
- `less`
    - move to top and bottom of file: `gg` and `G`
    - search: `/` then `n` and `N` for (next & previous selection)
- `more`
- `tail` and `head`
    - follow a log while it is written to: `tail -f /var/log/apache2/access.log`

---
## Text Manipulation
- `grep`
    - filter for lines in specific file: `grep "authenticating /var/log/apache2/access.log`
    - Recursive, case insensitive grep: `grep -R -i "PermitRootLogin" /etc/*`
    - find all lines that contain a specific string: `cat /var/log/apache2/access.log | grep '0.0.0.0' | wc -l`
    - use `-v` to find the inverse line that do NOT contain a specific string: `cat /var/log/apache2/access.log | grep -v '0.0.0.0' | wc -l` 
- `awk`
- `sed`
    - print line containing first occurance of a string: `sed -n '/STRING/p /var/log/apache/access.log | head -1`
    - print line containing last occurance of a string: `sed -n '/STRING/p /var/log/apache/access.log | tail -1`
- `jq`
- `cut
    - `cut -d' ' -f2`: `-d` delimter and `-f` field number will get the second word when piped from stdin
    - display the 10th word onwards (use space as the delimiter): `grep "authenticating" /var/log/auth.log| grep "root"| cut -f 10- -d" "`
- `sort`: sort order
- `uniq`: filter unique values

`.vimrc`
```
colorscheme evening
set relativenumber
set number
set colorcolumn=80
set tabstop=2 shiftwidth=2 expandtab
set backspace=indent,eol,start
highlight ColorColum ctermbg=0 guibg=lightgrey
```

---
## Archiving Files & Directories
gather files and directories into one place with tar: `tar -cvf myinits.tar /etc/init.d/`
compress the tarball with gzip: `gzip myinits.tar`
in 1 step: `tar -cvzf myinits.tgz /etc/init.d/`
    - `c` switch: create an archive file
    - `v` switch: print verbose output
    - `z` switch: compress the result
    - `f` switch: create the output tarball with the following filename
uncompress compressed tarball: `tar -xvf myinits.tgz`
    - `C` switch: specify a different directory to uncompress to


---
## Netowrking
- find local ip: `ip a`
- `ping`
- `telnet`
- `nmap [localhost or IP]`: show exposed ports
- `ss`:
    - more modern replacement for `netstat`
    - view open ports (both internal and exposed): `ss -tlp`
- `netstat`: See if the service is listening on the correct socket
    - view open ports: `netstat -tulpn`
    - o flag: show PIDs
- `traceroute`
- mtr
- `curl`
    - I flag: show header info only
    - L flag: follow redirects

### Firewalls
- netfilter
- `iptables`
    - view traffic rules: `iptables -L`
- `nftables`
- `ufw`
    - setup: `ufw allow http && ufw enable`

---
## Cron Scheduler
- [cron expressions](https://crontab.guru/)
- Install cron deb/ubuntu
    - `apt-get update && apt-get install cron`
    - `pgrep cron` or `pidof cron`
    - `systemctl start cron` or `service cron start` on old systems
- `crontab -e`
    - *NOTE: Make sure to add a NEWLINE at the end*
- different environment: cron passes a minimal set of environment variables to cronjobs
    - view them: `* * * * * env > /tmp/env.output`
- default log location: `/var/log/syslog`
- redirect cron logs to specified locations
    - e.g. `0 15 * * *    /home/user/daily-backup.sh >> /var/log/daily-backup.log 2>&1`

---
## SFTP
current working directory: `pwd`
upload files: `put path_to_local_file remote_file`
download files: `get path_to_remote_file local_file`

---
## Bash
- alias
    - `alias ll='ls -l'`
- cd -
- pwd
- ls -la
- cat / bat
## Git
- Delete remote branch: `git push -d <remote_name> <branch_name>`
- Delete local branch: `git push -D <branch_name>`
- Make git commands (e.g. pull) more verbose: `GIT_SSH_COMMAND="ssh -vv"`
- View files modified by git pull: `git diff --name-only HEAD@{0} HEAD@{1}`

### bash_profile vs. bashrc
- `.bash_profile` is executed at login, while `.bashrc` is executed on every shell instance
- can also use an `.ini` file and source it in the `.bash_profile` with a `source ~/my.ini`

`.bash_profile` example:
```
export PS1="\[\033[36m\]\u\[\033[m\]@\[\033[32m\]\h:\[\033[33;1m\]\w\[\033[m\]\$ "
#PS1='[${curr_host}@`if [[ "$mh" = "$PWD" ]]; then echo "$PWD"; else echo "~$    {PWD#$ih}"; fi`] $ '

INFA_HOME=/app/infa/Informatica/10.2.0
JAVA_HOME=$INFA_HOME/java
PATH=$PATH:$HOME/bin:$INFA_HOME/server/bin:$JAVA_HOME/lib
PYTHONPATH=$PYTHONPATH:/app-san/infa/infa_shared/Scripts/dimpypkg
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$ORACLE_LIBS:$INFA_HOME/server/bin:$JAVA_HOME/lib:$ODBCHOME/lib:$PWX_HOME
#LD_LIBRARY_PATH=$INFA_HOME/server/bin

export INFA_HOME
export JAVA_HOME
export PATH
export PYTHONPATH
export LD_LIBRARY_PATH
. ./my.ini
```

`my.ini` example"
```
#!/bin/sh
ih=/app-san/infa/infa_shared
curr_host=`echo $HOSTNAME | cut -d'.' -f 1`

alias lp='ls -alp'
alias lpt='ls -alptr | tail'
alias lpd='ls -alp | grep ^d'
alias lps='ls -alpSr | tail'

alias cache='cd $ih/Cache'
alias lkp='cd $ih/LkpFiles'
alias log='cd $ih/log'
alias temp='cd $ih/Temp'
alias param='cd $ih/ParamFiles'
alias scripts='cd $ih/Scripts'
alias slog='cd $ih/SessLogs'
alias src='cd $ih/SrcFiles'
alias tgt='cd $ih/TgtFiles'
alias wlog='cd $ih/WorkflowLogs'
alias infa='cd $INFA_HOME'
```

### Shell Scripting
#### Shebang
`#!/usr/bin/env bash`
#### Notes
- run the shell script as a custom command, such as `topattackers`
- write the shell script and make it executeable: `chmod +x topattackers`
- run it with `./topattackers`
- allow for it to be run anywhere throughout the system by moving it somewhere the PATH variable sees it
    - best place to store it is where manually installed software lives: `sudo mv ./topattackers /usr/local/bin/topattackers`
- now the script can be called by simply typing the command `topattackers`

#### Common commands
- print to screen: `echo` follow by a string or variable
- get user input: `read` followed by the variable name the input will be stored to
- use a variable with `$varname`
- make program wait: `sleep` followed by an int for the seconds
- write to file: `echo $varname > text.txt`
    - `>` will overwrite output to the following filename
    - `>>` will append output to the following filename

#### Script arguments
- `$0`
- `$1, $2` etc.
#### Functions


---
## Distros
- Servers: Debian
- Daily Driver: Ubuntu & Arch
- Business: CentOS

---
## Notes
- https://www.linode.com/docs/tools-reference/linux-system-administration-basics/


#### .bashrc vs. .bash_profile
.bash_profile is executed for login shells, while .bashrc is executed for interactive non-login shells.

When you login (type username and password) via console, either sitting at the machine, or remotely via ssh: .bash_profile is executed to configure your shell before the initial command prompt.

But, if you’ve already logged into your machine and open a new terminal window (xterm) then .bashrc is executed before the window command prompt. .bashrc is also run when you start a new bash instance by typing /bin/bash in a terminal.