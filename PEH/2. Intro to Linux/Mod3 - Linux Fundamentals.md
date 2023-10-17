Linux is an OS which sits in the middle of our hardware and users

Unix vs Linux
  Unix was first developed for multi-user and multi-tasking in mid-1970 in Bell labs by AT&T, GE and MIT 
  Then born Linux in 1991 by Linux Torvalds
  Linux is mostly free and open-source

Kernel:
  o The core of the UNIX system. Loaded at system start up (boot). Memory-resident control
  program.
  o Manages the entire resources of the system, presenting them to you and every other user
  as a coherent system. Provides service to user applications such as device management,
  process scheduling, etc.
  o Example functions performed by the kernel are:
    Managing the machine's memory and allocating it to each process.
    Scheduling the work done by the CPU so that the work of each user is carried out as efficiently as is possible.
    Accomplishing the transfer of data from one part of the machine to another
    Interpreting and executing instructions from the shell
    Enforcing file access permissions

Shell:
  o Whenever you login to a Unix system you are placed in a shell program. The shell's
  prompt is usually visible at the cursor's position on your screen. To get your work done,
  you enter commands at this prompt.
  o The shell is a command interpreter; it takes each command and passes it to the operating
  system kernel to be acted upon. It then displays the results of this operation on your
  screen.
  o Several shells are usually available on any UNIX system, each with its own strengths and
  weaknesses.
  o Different users may use different shells. Initially, your system adminstrator will supply a
  default shell, which can be overridden or changed. The most commonly available shells are:
    Bourne shell (sh)
    C shell (csh)
    Korn shell (ksh)
    TC Shell (tcsh)
    Bourne Again Shell (bash)

###### Linux file types:
  -     Regular file
  d     Directory
  l     Link
  s     Socket 
  p     Name pipe
  b     Block device 

###### Changing password:
  being a root user we can change any user's password but a normal user can only change its own password
  passwd <username>
  passwd

Creating multiple files with same name convention with different name:
  touch abcd{1..9}-xyz

Wildcard:
  * => all
  ? => one character
  [] => range of values 

  ls -l abcd*
  ls -l a?cd*
  rm *[cd]*  - delete every file which contains c or d

Soft & hard links:
  inode => Pointer or number of file on the hard disk
  Soft link => link will be removed if file is removed or renamed
  Hard link => deleting renaming or moving the original file will not affect the hard link
  - ln (hard)
  - ln -s (soft)

File Permissions:
  chmod g-w spider (remove )
  chmod g+w spider
  chmod u+x spider
  chmod o-x spider
  chmod a-r spider (remove all read permissions)

  chmod u+r,g+wx file.txt
  chmod o=r-- file.txt 

  Octal mode:
    0 - no (---)
    1 - execute (--x)
    2 - write (-w-)
    3 - execute, write (-wx)
    4 - read (r--)
    5 - read, execute (r-x)
    6 - read, write (rw-)
    7 - All (rwx)

There are two owners of the file or directory:
  User & group
  chown & chgrp
  Recursive ownership change should include -R 

  chown root file1.txt 
  chgrp root file1.txt 
  chown crypto:crypto file1.txt (user:group)
  
ACL:
  Acl provides an additional, more flexible permission mechanism for file systems. It is designed to assist with UNIX file permissions. Acl allows you to give permissions for any user or group

  Use of acl:
    Think of a scenario in which a particular user is not a member of group created by you but you still want to give some read/write access, how can we do it without making user a member of group. Acl comes into play

  To add permission for user:
    setfacl -m u:user:rwx /path/to/file
  To add permissions for a group:
    setfacl -m g:group:rwx /path/to/file 
  To allow all files or dirs to inherit ACL entries from the dir it is within
    setfacl -Rm "entry" /path/to/dir 

  To remove a specific entry:
    setfacl -x u:user /path/to/file (for a specific user)
  To remove all entries:
    setfacl -b /path/to/file (for all users)

  Get all permissions of file:
    getafcl file1 
  Note:
    As we assign the ACL permission to file/dir, it add + sign at the end of the permission
    Setting w permission with ACL does not allow to remove a file 

Pipes:
  Pipe is used by the shell to connect the output of one command directly to the input of another command 
  
  echo "sfgljjdfg " | tee file1

File maintenance commands:
  cp, mv, rm 
  mkdir, rmdir or rm -r 
  chown, chgrp, chmod  

Text processing commands:
  cut, awk, grep & egrep, sort, uniq, wc 

  cut: 
    cut is a command line utility that allows us to cut parts of lines from specified files or piped data and print the result to standard output. It can be used to cut parts of a line by delimiter, byte position, and character

    cut filename              =>Does not work
    cut --version             =>Check version
    cut –c1 filename          =>List one character
    cut –c1,2,4               =>Pick and chose character
    cut –c1-3 filename        =>List range of characters
    cut –c1-3,6-8 filename        =>List specific range of characters
    cut –b1-3 filename            =>List by byte size
    cut -d: -f 6 /etc/passwd      =>List first 6th column separated by :
    cut -d: -f 6-7 /etc/passwd    =>List first 6 and 7th column separated by :
    ls –l | cut –c2-4             =>Only print user permissions of files/dir

  awk:
    awk is a utility/language designed for data extraction. Most of the time it is used to extract fields from a file or from an output

    awk '{print $1}' file                 =>List 1st field from a file
    ls –l | awk '{print $1,$3}'           =>List 1 and 3rd field of ls –l output
    ls –l | awk '{print $NF}'             =>Last field of the output
    awk '/Jerry/ {print}' file            =>Search for a specific word
    awk -F: '{print $1}' /etc/passwd      =>Output only 1st field of lines of /etc/passwd
    echo "Hello Tom" | awk '{$2="Adam"; print $0}'     =>Replace words field words
    cat file | awk '{$2=“Imran"; print $0}'         =>Replace words field words
    awk 'length($0) > 15' file           => Get lines that have more than 15 byte size
    awk '{if($1 =="crypto") print $0}' /etc/passwd  =>Get the field matching crypto in /home/crypto
    ls -l | awk '{print NF}'       =>Number of fields.

  grep:
    Global Regular Expression Print
    processes text line by line and prints any lines which match a specified pattern

    grep keyword file         => Search for a keyword from a file
    grep –c keyword file      => Search for a keyword and count
    grep –i KEYword file      => Search for a keyword ignore case-sensitive
    grep –n keyword file      => Display the matched lines and their line numbers
    grep –v keyword file      => Display everything but keyword
    grep keyword file | awk '{print $1}'  =>Search for a keyword and then only give the 1st field
    ls –l | grep Desktop     =>Search for a keyword and then only give the 1st field

    egrep -i "crypto|root" passwd  => used to search two or more patterns 

  sort & uniq:
    Sort command sorts in alphabetical order
    Uniq command filters out the repeated or duplicate lines

    sort file             =>Sorts file in alphabetical order
    sort –r file          =>Sort in reverse alphabetical order
    sort –k2 file         =>Sort by field number (sort by 2nd column)

    uniq file             =>Removes duplicates
    sort file | uniq       =>Always sort first before using uniq their line numbers
    sort file | uniq –c     =>Sort first then uniq and list count
    sort file | uniq –d     =>Only show repeated lines.

  sed:
    sed 's/Kenny/Lemmy/g' file1.txt       => Replace Kenny with Lemmy gloabally (g option)
    sed -i 's/Kenny/Lemmy/g' file1.txt    => to save the changes in file
    sed 's/cortanza//g' file1.txt         => remove that word
    sed '/cortanza/d' file1.txt           => delete the line which contains keyword
    sed '/^$/d' file1                     => Remove empty line
    sed '1,2d' file1                      => Remove first 2 lines 
    sed 's/\t/ /g' file1                  => Replace tab with one space
    sed -n 12,18p file1                   => Print 12 to 18 lines 

  wc:
  The command reads either standard input or a list of files and generates: newline count, word count, and byte count

    wc file          => Check file line count, word count and byte count
    wc –l file       => Get the number of lines in a file
    wc –w file       => Get the number of words in a file
    wc –c file       => Get the number of characters in a file

  diff (Line by line)
    diff file1 file2
  cmp (Byte by byte)
    cmp file1 file2

Compressing & decompressing:
  tar, zip, gzip

  tar:
    tar -cvf archive.tar foo bar  # Create archive.tar from files foo and bar.
    tar -cvf archive.tar .  # Create archive.tar from files present in currrent directory
    tar -tvf archive.tar         # List all files in archive.tar verbosely.
    tar -xvf archive.tar          # Extract all files from archive.tar.

  gzip:
    gzip hello.tar (compressing it agn) size gets reduced
    gzip hello.tar.gz (decompressing)

  truncate:
    truncate is used to shrink or extend the size of a file to a specified size 
    
    truncate -s 10 filename (on reducing, it alters the content of the file)

Combining and splitting files:
  Multiple files can be combined into one 
    cat file1 file2 file3 > file4 
  One file can be split into multiple files 
    split file4 
    split -l 300 file.txt childfile (split file.txt into 300 lines per file and output to childfileaa, childfileab, childfileac)