# Linux
- command docs: `man find`
- `apt-get update && apt-get upgrade`
- `yum update`

## Configuration
- Deb hostname: edit `/etc/hostname` & rename `/etc/host` entries
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
- delete user: `userdel <user>`
- assign group: `usermod -aG <group> <username>`
- give sudo: 
    - Deb: `usermod -aG sudo <user>`
    - Fed: `usermod -aG wheel <user>`
- copy ssh key: `ssh-copy-id -i $HOME/.ssh/id_rsa.pub <user@ip>`

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
- executable location: `which vim`
- location of file within a specific dir containing name: `find /var -name access.log`
- files that have been edited in the last 3 hours: `find /home -mtime -3`
- locate file anywhere in the filesystem: `locate access.log`
    - requires updated db: `sudo updatedb`


---
## File System Management
- check disk usage: `ncdu`
- ln -s
- cp -R
- mkdir -p
- touch
- mv
- rm
- scp
- tree

#### Filepaths
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

---
## Text Manipulation
- grep
    - Recursive, case insensitive grep: `grep -R -i "PermitRootLogin" /etc/*`
- awk
- sed / jq
- `cut -d' ' -f2`: `-d` delimter and `-f` field number will get the second word when piped from stdin

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
## Package Management
#### apk
#### apt
- view installed packages: `apt list --installed`
- specific packge: `apt list -a pkgNameHere`
#### dpkg
#### yum
#### pacman

---
## Netowrking
- `nmap [localhost or IP]`: show exposed ports
- ping
- traceroute
- `netstat`: See if the service is listening on the correct socket
    - o flag: show PIDs
- telnet
- mtr
- curl
    - I flag: show header info only
    - L flag: follow redirects

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

#### Script arguments

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