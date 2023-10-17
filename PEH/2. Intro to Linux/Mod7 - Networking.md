Interface detection 
Assigning an IP address 
Interface config files:
  /etc/nsswitch.conf 
  /etc/hostname
  /etc/resolv.conf 

Network commands:
  ping, ifconfig, ifup or ifdown, netstat, tcpdump 

  netstat -rnv 
  netstat -ano
  tcpdump -i eth0

Network interface card:
  ethtool ens160          => gives information abt the NIC 


NIC bonding (network bonding):
  It can be defined as the aggregation or combination of multiple NIC into a single bond interface 
  Its main purpose is to provide high availablility and redundancy 

Network configuration:
  nmcli                       => to make network configuration changes through command line 
  nmtui                       => make changes through a text user interface
  nm-connection-editor        => it can be accessed through desktop or console 
  GNOME settings              => desktop menu

  nmcli:
    nmcli con show      => shows connection
    nmcli dev show      => shows nic connected to machine

  Using nmcli to configure static ip 
    nmcli connection modify ens809 ipv4.addresses 10.256.1.211/24
    nmcli connection modify ens809 ipv4.gateway 10.256.1.1
    nmcli connection modify ens809 ipv4.method manual
    nmcli connection modify ens809 ipv4.dns 8.8.8.8
    nmcli connection down ens809 && nmcli connection up ens809

    ip address show ens809

  Adding secondary ip using nmcli 
    nmcli device status 
    nmcli connection show --active 
    ifconfig 
    nmcli connection modify ens809 +ipv4.addresses 10.0.0.211/24
    nmcli connection reload 
    systemctl reboot 
    ip address show  

Downloading files:
  wget, curl 

  wget https://apache.com/latest.zip -O lat.zip 
  curl https://apache.com/latest.zip -o lat.zip 
  
ftp (port 21):
  It is a standard network protocol that is used to share files between the client and server 

  Install and configure ftp:
    become root
    rpm -qa | grep vsftpd
    yum install vsftpd
    nano /etc/vsftpd/vsftpd.conf 

    -- find the lines and make changes 
    ## disable anonymous login 
    anonymous_enable=NO 
    # Uncomment these lines
    ascii_upload_enable=YES
    ascii_download_enable=YES
    # Edit welcome msg 
    ftpd_banner=Welcome to unix ftp centos server 
    # Add this at the end of this file 
    use_localtime=YES 

    systemctl start vsftpd
    systemctl enable vsftpd
    systemctl stop firewalld

  On client:
    yum install ftp 
    touch kruger.file
    ftp 192.263.35.3 
    Enter username and password of tht server 
    bi (to enter in binary mode)
    hash (to show more information about the transfer)
    put kruger.file 
    bye 

scp (port 22):
  helps to transfer files securely from a local to a remote host. Similar to ftp but it adds security and authentication

  copy file from localhost to a remote host:
    scp linpeas.sh crypto@192.168.1.342:/home/crypto 

  copy file from remote host to a localhost:
    scp crypto@192.168.1.342:/home/crypto/hello.sh . 
    scp -r crypto@192.168.1.342:/home/crypto/ .       (recursively download all files of that directory)

rsync (port 22):
  rsync is a utility for efficiently transferring and synchronising files within the same computer or to a remote computer by comparing the modification times and sizes of files 
  rsync is lot faster than scp or ftp 

  yum install rsync (centos/redhat)
  apt-get install rsync (debian/ubuntu)

  rsync a file on a local machine:
    tar cvf backup.tar . 
    rsync -zvh backup.tar /tmp/backup 

  rsync a directory on a local machine:
    rsync -azvh /home/crypto /tmp/backup/ 

  rsync a file to a remote machine:
    rsync -azv backup.tar crypto@192.168.0.106:/tmp/backup/

  rsync a file from a remote machine:
    rsync -azvh crypto@192.168.0.106:/tmp/backup/ /tmp/backups