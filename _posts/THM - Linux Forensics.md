# TryHackMe 

---

## Linux Forensics

### Task 3

> In the attached VM, there is a user account named tryhackme. What is the uid of this account?

```bash
cat /etc/passwd | grep tryhackme
```

`1001`

> Which two users are the members of the group `audio`?

```bash
cat /etc/group | grep audio
```

`ubuntu,pulse`

> A session was started on this machine on Sat Apr 16 20:10. How long did this session last?

```bash
sudo last -f /var/log/wtmp
```

`01:32`

### Task 4 

> What is the hostname of the attached VM?

```bash
cat /etc/hostname
```

`Linux4n6`

> What is the timezone of the attached VM?

```bash
cat /etc/timezone
```

`Asia/Karachi`

> What program is listening on the address 127.0.0.1:5901?

```bash
netstat -natp | grep 127.0.0.1:5901
```

`xtigervnc`

> What is the full path of this program?

```bash
ps aux | grep Xtigervnc
```

`/usr/bin/Xtigervnc`


### Task 5

> In the bashrc file, the size of the history file is defined. What is the size of the history file that is set for the user Ubuntu in the attached machine?

```shell
cat /etc/bash.bashrc | grep history
echo $HISTFILESIZE```

`2000`

### Task 6
> The user tryhackme used apt-get to install a package. What was the command that was issued?

```bash
cd /home/tryhackme
sudo cat .bash_history | grep apt-get
```

`sudo apt-get install apache2`

> What was the current working directory when the command to install net-tools was issued?

```bash
cat /var/log/auth.log* |grep -i COMMAND|grep net-tools|tail
```

`/home/ubuntu`

### Task 7
> Though the machine's current hostname is the one we identified in Task 4. The machine earlier had a different hostname. What was the previous hostname of the machine?

```shell
cat /var/log/syslog* | grep "hostname changed"
```

`tryhackme`









