# TryHackMe 

---

## NIS - Linux Part I

### Task 1

> What is the user you are logged in to in the first room of Linux Fundamentals Part 1?

`tryhackme`

> What badge do you receive when you complete all the Linux Fundamentals rooms?

`cat linux.txt`

Connect via SSH to the given ip address: `ssh chad@<ip_address>`
Input password: `Infinity121`


### Task 2

You can use `ls --help` or `man ls` to see the basics manual in the terminal.

> How do you run the ls command?

`ls`

> How do you run the ls command to show all the files inside the folder?

`ls -a`

> How do you run the ls command to not show the current directory and the previous directory in the output? (almost everything)

`ls -A`

From the --help we know that: 
`-A, --almost-all           do not list implied . and ..`

> How do you show the information in a long listing format using ls?

`ls -l`

> How do you show the information in a long listing format using ls?

`ls -h`

> How do you do a recursive ls?

`ls --recursive`

`ls -R` also works.

> How many files did you locate in the home folder of the user?(non-hidden and not inside other folders)

`13`

### Task 3

> What is the content of cat.txt?

At /home/chad/ we use:

`cat cat.txt`

> What is the content of tac.txt?

`tac tac.txt`

> What is the content of head.txt?

`head head.txt`

> What is the content of tail.txt?

`tail tail.txt`

> What is the content of the xxd.txt?

`xxd xxd.txt`

> What is the content of base64.txt?

`base64 base64.txt | base64 --decode`


### Task 4

> How many .txt files did you find in the current folder?

`8`

You can check it in this way:
`find . -type f -name "*.txt"
<command><current folder><type file><extension .txt>`

> How many SUID files have you found inside the home folder?

`0`

`SUID is a special file permission for executable files which enables other users to run the file with effective permissions of the file owner. Instead of the normal x which represents execute permissions, you will see an s (to indicate SUID) special permission for the user.`

You can find those files:

`$ find . -perm /4000`

### Task 5

> How many times does the word "hacker" appear in the grep files? (including variations)?

`15`

`cat grep1.txt grep.txt | grep hacker`

### Task 6

> Is the user allowed to run the above command? (Yay/Nay)

`Nay`

### Task 7

> No answer needed.

### Task 8

> What command would you use to echo the word "Hackerman" ?

echo "Hackerman"`

### Task 9

> How would you read all files with extension .bak using xargs?

`find / -name *.bak -type f print | xargs /bin/cat`

### Task 10

`No answer needed`

### Task 11

> How would you grab the headers silently of https://tryhackme.com but grepping only the HTTP status code?

`curl -l -s https://tryhackme.com | grep HTTP`

### Task 12

> What command would you run to get the flag.txt from https://tryhackme.com/?

`wget https://tryhackme.com/flag.txt`

> What command would you run to download recursively up to level 5 from https://tryhackme.com?

`wget -r -l -5 https://tryhackme.com/`

### Task 13

> What is the flag from the tar file?

`tar -xf tarball.tar`

`cat flag.txt`

`THM{C0FFE1337101}`

### Task 14

> What is the content of gzip.txt?

`gzip -d gzip.txt.gz`

`cat gzip.txt`

`THM{0AFDECC951A}`

### Task 15

> What is the flag inside the 7zip file?

`7z x zip.7z`

`cat 7zip.txt`

`THM{526accdf94}`

### Task 16

> What is the content of binwalk.txt?

`THM{af5548a12bc2de}`
