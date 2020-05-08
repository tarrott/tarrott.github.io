# Linux
- `apt-get update && apt-get upgrade`
- `yum update`

## Configuration
- No password configured: `sudo passwd`
- `hostname -f`
- `dpkg-reconfigure tzdata / timedatectl set-timezone`
- fdisk

---
## System Diagnostics
- `free -h`
- top / htop
- df -h
- vmstat
- ps -ef
- pgrep / pidof

---
## File System Management
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
- awk
- sed / jq

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
#### apt / apt-get
#### dpkg
#### yum
#### pacman

---
## Netowrking
- `nmap`: show exposed ports
- ping
- traceroute
- netstat
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
## Bash
- alias
    - `alias ll='ls -l'`
- cd -
- pwd
- ls -la
- cat / bat

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