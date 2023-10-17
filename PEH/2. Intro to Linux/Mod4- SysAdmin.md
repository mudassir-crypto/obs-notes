vi (text editor):+
  i – insert mode 
  Esc – Escape out of any mode
  r – replace
  dd – delete the line
  :q! – quit without saving
  :wq! – quit and save

User account management:
  useradd, groupadd, userdel, groupdel, usermod 

  useradd –g superheros –s /bin/bash –c “user description” –m –d /home/spiderman spiderman
  userdel -r spider                   => remove the user as well as its home directory
  usermod -aG superhero spider         => adding user to a group superhero 
  chgrp -R superhero /home/spider     => changing group perm of the spider home dir 

  chage: data is inserted into shadow file
    default config file - /etc/login.def 
    used to set password aging 

    chage -m mindays -M maxdays -W warnbeforeExpireday -I daysafterwhichpaasswordbecomesInactive user

Switch users and sudo:
  file - /etc/sudoers
  change permission of group or user to run sudo 
    visudo (and edit the file)

Monitor users:
  who, last, pinky, id 
  who       => prints information about users who are currently logged in
  last      => used to display a list of users who have previously logged in to the system
  pinky     => prints the user information just like who 

Talking to users:
  users  => list the users that are logged in
  wall => writes a message to all users that are logged in
  write crypto => sends the msg to the user specified 
  
Linux account authentication:
  Local accounts
  Domain/directory accounts 

  Windows = Active Directory
  Linux = ldap?

  ldap(light weight directory access protocol) is a protocol used by linux, windows, mac to authenticate against a directory 

  Difference between Active Directory, LDAP, IDM, WinBIND, OpenLDAP:
    • Active Directory = Microsoft
    • IDM = Identity Manager (used in enterprise level red hat, manages the sysadmins)
    • WinBIND = Used in Linux to communicate with Windows machine of active directory(Samba)
    • OpenLDAP (open source)
    • IBM Directory Server
    • JumpCloud
    • LDAP = Lightweight Directory Access Protocol


System utility commands:
  date => shows date 
  uptime => shows time, how long the system has been running, no. of users currently logged in, cpu load 
  hostname
  uname -a 
  which pwd => where that command is located
  cal => calendar
    cal 2018 (all months of 2018)
    cal 9 1977 (september of 1977)
  bc => binary calculator

Process, jobs, scheduling:
  Application   eg. NFS, rsyslog, firefox
  Script        eg. adduser, cd, pwd
  Process       Applications have multiple processes with its process id 
  Daemon        Its a process that runs continuously 
  Thread        Every process could have multiple threads associated with it 
  Job           run a service or process at a schedule time

  systemctl, ps, top, kill, crontab, at 

  systemctl:
    systemctl start|stop|status servicename.service
    systemctl enable|disable servicename.service (these services starts at boot time)
    systemctl restart|reload servicename.service (when we make config changes)
    systemctl list-units --all (all the service which are enabled or not enabled)

    firewalld.service (tried this one on the above commands)

    To add a service under systemctl management:
      Create a unit file in /etc/systemd/system/servicename.service

    To control system with systemctl:
      systemctl poweroff (turns off the machine)
      systemctl halt
      systemctl reboot  (restarts the machine)

  ps:
    Process status displays all the current running processes in the linux system 

    ps            => shows the process of the current shell 
    ps -ef        => shows all running process in full format listing
    ps aux        => shows all running process in BSD format 
    ps -u root    => shows process running by username
  
  top:
    It shows linux processes in real time 
    Top command refreshes the information every 3 seconds

    top -u crypto     => task process owned by user 
    top then press c  => shows commands absolute path
    top then press k  => kill a process by PID within top session

  kill:
    It is used to terminate process manually 

    kill PID         => kill process identified bt process id 
    kill -9 PID      => forcefully kill the process 
    kill -15 PID     => gracefully kill the process 

  pkill is used to terminate process by name 
  killall kills the process as well as its child processes 

  crontab:
    crontab is used to schedule tasks

    crontab -e          => Edit the crontab table 
    crontab -l          => List the crontab entries
    crontab -r          => remove the crontab entries 
    crond               => crontab daemon
    systemctl status crond     => To manage the crond service

      min  hour  dayOfMonth  month  dayOfWeek  <command to execute>   

      min(0-59)
      hour(0-23)
      dayOfMonth(1-31)
      month(1-12)
      dayOfWeek(0-6) => sunday to saturdaye, 7 is also sunday on some

    Create a crontab emntry by scheduling a task:
      crontab -e 
      schedule, echo "This is my first crontab entry" > crontab-entry 

      Run it at 16:21 in october every day of month and week 
        crontab -e 
        21 16 * 10 * echo "This is my first crontab entry" > crontab-entry
        :wq!
        cat crontab-entry 
        crontab -l 
        crontab -r (entry will be removed) 

  at:
    at is like crontab which allows us to schedule a job but only once 
    when the command is ran it wwill enter interactive mode and we can get out by pressing Ctrl+D 

    at HH:MM PM           => Schedule a job 
    atq                   => List the at entries 
    atrm #                => remove at entry 
    atd                   => at daemon/service that manages scheduling

    Create at entry by scheduling a task:
      at 4:45PM   -> enter 
      echo "first at entry" > /home/crypto/hello.txt
      Ctrl + D 
    
    at 2:45AM 101621        => Schedule a job to run on Oct 16, 2021 at 2:45am
    at 4PM + 4 days         => Schedule a job to run at 4PM four days from now 
    at now + 5 hours        => Schedule a job to run five hours from now 
    at 8:00AM Sun           => Schedule a job to run at 8am sunday 
    at 10:00AM next month   =>

  By default, there are 4 different types of cronjobs
    • Hourly
    • Daily
    • Weekly
    • Monthly
  • All the above crons are setup in
    • /etc/cron.___ (directory)
  • The timing for each are set in
    • /etc/anacrontab -- except hourly
  • For hourly
    • /etc/cron.d/0hourly

Process management commands:
  bg, fg, jobs 

System Monitoring:
  top         => shows dynamic real time processes
  df          => gives us disk partition information
  dmesg       => gives information abt our system hardware issue
  iostat      => used for monitoring system input/output statistics for devices and partitions
  netstat     => displays the contents of various network-related data structures for active connections
  free        => displays the total amount of free space available
  cat /proc/meminfo
  cat /proc/cpuinfo

Log monitoring:
  Log directory - /var/log 
    boot          => whenever system starts, it rewrites the log into this file 
    chrony        => its a service and the logs are stored in this file 
    cron 
    maillog       => contains mail logs 
    secure        => records all the logging in and logging out activities 
    messages      => sysadmin checks this file whenever something goes wrong 
    httpd         =>

System maintenance:
  shutdown 
  init 0-6
  reboot 
  halt        => shuts off the machine just like hitting a power button on machine
  
Changing hostname:
  file - /etc/hostname 
  hostnamectl set-hostname newhostname

System information:
  uname
  dmidecode

Difference between 32-bit and 64-bit is the number of calculations per second they can perform 
Multiple cores allow for an increased no. of calculations per second which can increase processing power
  arch    => gives the architecture of os

  script log-activity.log  => The script command stores terminal activities in a log file that can be named by a user, when a name is not provided by a user, the default file name, typescript is used

Recover root password:
  Restart => Edit grub => change password => reboot 

  reboot => press e on the option of OS => move below and find 'ro'
  => replace it with 
      rw init =/sysroot/bin/sh 
      ctrl x          (this will start the machine in single user mode)
      chroot /sysroot 
      passwd root
      touch /.autorelabel
      exit 
      reboot

What is SOS Report? It will collect all the config and log files 
  • Collect and package diagnostic and support data
  Package name
    sos-version
  command:
    sosreport

Environment variable:
  set of defined values to build an environment 

  env                   => view all env variables
  echo $SHELL           => view SHELL variable 
  export TEST=12        => set env variable 

  set env variable permanently:
    vi .bashrc
    TEST='1232'
    export TEST
  
  set global env variable permanently:
    vi /etc/profile or /etc/bashrc
    TEST='123'
    export TEST 

Special permissions:
  There are 3 additional permissions in Linux
    setuid: bit tells Linux to run a program with the effective user id of the owner instead of the
    executor. (e.g. passwd command) → /etc/shadow
    setgid: bit tells Linux to run a program with the effective group id of the owner instead of the
    executor. (e.g. locate or wall command)
      Please note: This bit is present for only files which have executable permissions
    sticky bit: a bit set on files/directories that allows only the owner or root to delete those files
        rwt 
        Other users are not permitted to delete the content of directory which has sticky bit (t)

  chmod u+s xyz.sh        => assign special permission at user level
  chmod g+s xyz.sh        => assign special permission at group level

  chmod u-s, g-s xyz.sh     => remove special permission

  Find executable with setuid and setgid permission
    find / -perm /6000 -type f 2>/dev/null
