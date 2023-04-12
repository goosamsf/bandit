bandit6 : `P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU`

---
### bandit Level6 --> Level7
Prompt 
The password for the next level is stored somewhere on the server and has all of the following properties:

    owned by user bandit7
    owned by group bandit6
    33 bytes in size

- Basic Usage of `find` command
```
$ find WHERE_TO_LOOK CRITERIA ACTION

eg)
$ find /home/jun -name "*.py" -print
```

==Solution==
>1. Go to root directory.
>2.  run following command
```
find . -size 33c -a -user bandit7 -print 2>dev/null
```
Explanation :
 1. Find from current directory
 2. size is 33bytes  &&  owned by username 'bandit7'
 3.  print but redirect the error messages (FD #2) to /dev/null, so you don't have to see them on screen.
 	==quick note== `/dev/null` can be used for any unwanted output from program/command

> 3. Now the output narrows down to
```
./etc/bandit_pass/bandit7
./var/lib/dpkg/info/bandit7.password
```
>4.  Try to `cat` each one
>5.  `./var/lib/dpkg/info/bandit7.password` contains answer
>

bandit7, `z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S` done.

---
### bandit Level7 --> Level8

Problem Prompt:
>The password for the next level is stored in the file data.txt next to the word millionth

- Basic Usage : `grep`
```
$ grep PATTERN FILENAME
```

==Solution==
>1. Run following command :
>	`grep millionth data.txt`
>2. This will simply give you  the output :
```
millionth	TESKZC0XvTetK0S9xNwm25STk5iWrBvP
```

bandit8, `TESKZC0XvTetK0S9xNwm25STk5iWrBvP` done.

---

### bandit Level8 --> Level9

Problem Prompt:
>The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

- `sort` : sort data
```
$ sort data.txt
//If there are duplicate lines, ther will be aligned together.
```

- `uniq` : Print duplicate lines once(without any flag)
- `uniq -u` :  uniq with `u` option prints unique lines.

==Solution==
>1. Run following command :
```
$ sort data.txt | uniq -u
```
- Explanation 
	- "sort data in data.txt file and print non-duplicate line"
>2. This will give you following output:
> 	

bandit9, `EN632PlfYiZbn3PhVK3XOGSlNInNE00t` done. 	

---

### bandit Level9 --> Level10

Problem Prompt : 
>The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

==Solution==
>1. Run following command
```
$ grep -aow '\w\{10,40\}' data.txt
```
 
 Explanation
 - `-a` option lets grep treat all files as ascii file(should be done on binary file)
 - `-o` option displays only matched pattern(normally displays the line that contain matched pattern)
 - `-w` option lets grep search for as a word
 - `\w\{10,40\}` : regular expression followed by quantifier : word length between 10 and 40. When quantifier used in grep , `\` escape character need  before each braces.

bandit10, `G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s` done.

---
### bandit Level10 --> Level11

Problem Prompt:
>The password for the next level is stored in the file data.txt, which contains base64 encoded data

==Basic Usage : base64==
`base64 -d INPUTFILE` 

Simple,
It will give you :
`The password is 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM`
bandit11, `6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM`  done.

---

### bandit Level11 --> Level12
Problem Prompt:
>The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

- ROT13 and its implementation using `tr` and `regexp`
- [ROT13](https://en.wikipedia.org/wiki/ROT13)
- [[tr]]

The answer is :
bandit12, `JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv` done.

---

### bandit Level12 --> Level13

Problem Prompt:
>The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

==Helpful Material==
- [Hexdump](https://en.wikipedia.org/wiki/Hex_dump)
- `xxd -r` : [[xxd]]

- `gunzip filename.gz` : decompress `.gz` compressed file

- `bzip2 -d` : decompress `.bz2` file

When given tar archive file(data6.bin.out: POSIX tar archive (GNU)),
extract using :
- `tar -xvf` : 

The answer is :
bandit13, `wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw` done.

---

### bandit Level13 --> Level14
Problem Prompt :
>The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on

[SSH/OpenSSH/Keys](https://help.ubuntu.com/community/SSH/OpenSSH/Keys)

[[SSH OpenSSH Keys]]

In summary, how to log in with private key in ssh :
```bash
$ ssh -i <filename> <username>@<host>
```
- `filename` is the file that contain private key
- `-i` flag lets `ssh` takes a file that contain private key.

==Solution==
`$ ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220`

- Now you are logged in bandit14 so you get to open bandit14 file that contain password.

`$ cat /etc/bandit_pass/bandit14`

Answer :
bandit14, `fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq` done.

---

### bandit Level14 --> Level15

Problem Prompt :
>The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

==Helpful Materials==
[[How the Internet works]]
- Servers are directly connected to Internet(Wire)
- Clients are connected to ISP(Internet Service Provider) and ISP provide Internet to Our local computer
[[What is Local host]]
[[nc or netcat]]

```bash
bandit14@bandit:~$ nc localhost 30000
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
Correct!
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
```
bandit15, `jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt` done.

---
### bandit Level15 --> Level16

Problem Prompt:
> The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.
>

[[Encrypting and Decrypting files with OpenSSL]]

[[What is openssl s_client]]

- This level is pretty much same as the last. But this time we need to connect through SSL which basically means encrypted communication.


```bash
$ openssl s_client -ign_eof -connect localhost:30001
```

Answer :
bandit16, `JQttfApK4SeyHwDlI9SXGR50qclOAil1` done.

---
### bandit Level16 --> Level17

Problem Prompt :
> The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
- [ ] 

[Port Scanner on Wikipedia](https://en.wikipedia.org/wiki/Port_scanner)
[[Port Scanner, nmap?]]

==Solution==
> 1. `nmap` and `ncat` to find right port :
> 	 `nmap localhost -p 31000-32000` will give you open port
> 	 brute force all open port by `ncat localhost --ssl <port#>`
> 2. One port give you private key
> 	- Copy that private key and paste into some file under `/tmp`
> 		(only `/tmp` allow user writing permission)
	3. `chmod 400 THATFILE` (Requirement of ssh secure login)
	4. Login 
		-`ssh bandit17@localhost -p 2220 /tmp/pvt16/pvt.key`
	* Note, All the online solutions doesn't specify port number in step 4 but I had to include port number in my machine. Spent too much time on this.
	5. Now you are `bandit17`
		You can do : `cat /etc/bandit_pass/bandit17`
   
Answer : 
bandit17, `VwOSWtCA7lRKkTfbr2IDh6awj9RNZM5e` done.

---

### bandit Level17 --> Level18

Problem Prompt :
>There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19

==Solution==
>1. Use `diff` to find out the different line.
```bash
bandit17@bandit:~$ diff passwords.new passwords.old
42c42
< hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg
---
> f9wS9ZUDvZoo3PooHgYuuWdawDFvGld2
```
`diff` :
The diff utility compares the contents of file1 and file2 and writes to
     the standard output the list of changes necessary to convert one file
     into the other.  No output is produced if the files are identical.

`<` : line that have been removed.
`>` : line that have been added.

bandit18,  `f9wS9ZUDvZoo3PooHgYuuWdawDFvGld2`  done.

---

### bandit Level18 --> Level19
Problem Prompt :
>The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

==Solution==
- `ssh` allows you to log into a machine remotely,

Key point, 
- Additionally, `ssh` also allows `remote execution` of commands by adding the commands after the `ssh` log in expression

```bash
$ ssh bandit18@bandit.labs.overthewire.org -p 2220 ls
```
will show you there is a `readme` file in home directory.

```bash
$ ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
```

now give you the answer :

bandit19,  `awhqfNnAbc1naukrpqDYcF95h7HoMTrC` done.

---

### bandit Level19 --> Level20

Problem Prompt :
>To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

[[What is Setuid]]

Key Point :
>file with `s` bit on(can be checked by `ls -l` command) indicates `setuid`
>
> Any commands executing with `./bandit20-do` will run as user `bandit20` instead of bandit19

==Solution==
```bash
$ ./bandit20-do cat /etc/bandit_pass/bandit20
```

Answer :
bandit20, `VxCazJaVykI6W36BkBU0mJTCM8rR95XT`  done.

---

### bandit Level20 --> Level21
Problem Prompt :
>There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

==Solution==

>1.  We know the bandit20 password. It can be found at `/etc/bandit_pass/bandit20`,  `cat` it.
>2. set up  a listener and send this password.
```bash
$ echo -n 'VxCazJaVykI6W36BkBU0mJTCM8rR95XT' | nc -lp 1234 &
```
- `echo` 's -n flag will remove any '\n' chracter in the message.
- `nc` : create arbitrary connections and listens(`-l` flag specifically do listen)
	`-p` flag let us specify flag.
- 1234 is an arbitrary port number 
- `&`  at the end of the command is used to specify that we want this command  to run in the background.( It will print `process id` in the stdout)
	- `jobs` command can check all the processes/ jobs on the system.

> 3. Now run given executable in home directory and specify the port number that we created above (`1234`)
```bash
$ ./suconnect 1234
```
- `suconnect` will take care of everything. We knew by the way, we were given this information what `suconnect` does, so all we had to do was to set up arbitrary connection that listen `bandit20` 's  password.

bandit21,  `NvEJF7oVjkddltPSrdKEFOllh9V1IBcq` done.

---

### bandit Level21 --> Level22

Problem Prompt :
>A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

==Solution==
> 1.  Look over `/etc/cron.d` : There is a file called `cronjob_bandit22` : Since we are in bandit21, this file might have useful information.
> 2. See the contents of `cronjob_bandit22`
```bash
$ cat /etc/cron.d/cronjob_bandit22

@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```
- This says every minute, shell script called `cronjob_bandit22.sh` is executed.
> 3. See the contents of the shell script
```bash
$ cat /usr/bin/cronjob_bandit22.sh
```

```bash
#!/bin/bash
# cronjob_bandit22.sh
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

- What it says : 
	-There is a file called ` t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`, change modes to `644`
	-Redirect bandit22's password into above file
	-Therefore, `t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv` file has the password for bandit22.

>4. cat `t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`
```bash
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

bandit22,  `WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff` done.

---


### bandit Level22 --> Level23
Problem Prompt: 
>A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

==Solution==
>1.  Look over `/etc/cron.d` : there is a file called `cronjob_bandit23`
>2. See the contents of the file :
```bash
$ cat /etc/cron.d/cronjob_bandit23

@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
```
>3. There is a shell script, look at the contents :
```bash
$ cat /usr/bin/cronjob_bandit23.sh

#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

- You need to be able to understand what this shell script does :
	1.  Variable can be defined using ` =$(command that prints to stdout)` For example, whatever the command whoami prints will be stored into a variable called `myname`
	2. Whatever `echo`ed is piped into `md5sum`
		- `md5sum` is hash function that takes string and return 16bytes(128bits)  md5sum( Precise definition : `md5sum` is a computer program that calculates and verifies 128-bit MD5 hashes) 
		- `cut` : can select certain portions of output : For example , 
			- `cut -d ' ' -f 1`  does , From the output, select first portion of the thing delimited by single white space.

> 4. Notice, if you run that shell script as it is, `myname` will get `bandit22` because you are in bandit22. And this result in `md5sum` value by `bandit22` which is not going to be `/tmp/$mytarget` that we are looking for.
	So run following command,
```bash
$ echo -n 'I am user bandit23' | md5sum | cut -d ' ' -f 1
```
This will give you : '8'ca319486bfbbc3663ea0fbe81326349'

> 5. Now run :
```bash
$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```
which will give you the password for bandit23 :

Answer :
bandit23, `QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G` done.

---

### Bandit Level 23--> Level 24

Problem Prompt:
>A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

==Solution==

- High level overview of the strategy.
> - There is a shell script in bandit24 that does :
> 	- if file was written by bandit23 run it for a minute and delete.
> - So as a bandit23 user, write a shell script that write bandit24's password into a file that bandit23 can access in the area where bandit24's shellscript running. 

> 1. See what you are given : `cat etc/cron.d/cronjob_bandit24`

```bash
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```

>2. See the contents of the shell script `cronjob_bandit24.sh`

```bash
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done

```

- You need to be able to understand what this does :
	1. set a `myname` variable : `bandit24`
	2. Go to the directory :  `/var/spool/bandit24/foo`
	3. `echo` some prompt

	4. For Loop :
		 for `i` in any name`.`any extension (all files)
				- check if `i` is current directory or parent directory
				- otherwise, 
					- `stat` command's '%U' option give you username
					- store username into a variable called `owner`
					- check if `owner` is `bandit23`
						- if it is, run it for 60seconds and delete it.

>3.  If we can place a shell script that was written by me(bandit23) into `/var/spool/bandit24/foo`, that shell script will be run by the `bandit24` authorization.
> So let's write shell script in `/tmp` directory and copy that into `/var/spool/bandit/foo`

```bash
$ mkdir /tmp/myshellscript
$ cd /tmp/myshellscript
$ vim sneakinto.sh

```

`sneakinto.sh`
```bash
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/myshellscript/get24pw

```

create a file `get24pw` into `myshellscript` directory
```bash
$ touch get24pw
```

Make all files under `myshellscript` accessible by anyone so that `bandit24` has no issue to access these files.
```bash
$ chmod 777 -R /tmp/myshellscript
```
above command make all files `rwxrwxrwx` 

>4. Now let's sneak into `/var/spool/bandit/foo`
```bash
cp sneakinto.sh /var/spool/bandit/foo
```

>5. By `cron` 's scheduling management, `bandit24`'s shell script will recognize `sneakinto.sh` in the `var/spool/bandit/foo` which was written by `bandit23` and run that shell script which is `cat` 24's password into a file that 23 can read.


Answer :
bandit24, `VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar` done.

---

### Bandit Level 24--> Level 25
Problem Prompt :
>A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
	You do not need to create new connections each time

==Solution==

- High level overview of the approach
	1. Write a shell script that writes password for bandit24 and each of 4-digits number from 0000 to 9999 and redirect output.
	2. Feed that redirected output into `nc`

>1 . Write shell script that containing some kind of loop to enumerate all possible combinations

```bash
mkdir /tmp/myforce
touch script.sh
```

`script.sh`
```bash
#!/bin/bash
for i in {0000..9999}
do
	echo "VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar $i"
done

```

>2. Make it executable
```bash
chmod 700 script.sh
```

>3.  Run it and redirect
```bash
./script.sh > combinations.txt
```

>4. Feed `combinations.txt` into `nc localhost 30002`
```bash
nc localhost 30002 < combinations.txt | grep -v Wrong!
```

Answer :
bandit25, `p7TaowMYrmu23Ol8hiZh9UvD0O9hpx8d` done.

---
### Bandit Level25 --> 26 --> 27

Problem Prompt : 
>logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.

==Solution==
> 1. We are given a private key, which it might look like very easy to get to the next level as it says in the problem prompt. But before doing that, let us look at the `bandit26`'s information on by :
```bash
cat /etc/passwd | grep bandit26
```
- `passwd` is a command that user can change the password but file : `/etc/passwd` is the file that contains user account information. For example running above command give you,
```bash
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```

It's weird. Normally that command will show :
```bash
bandit25:x:11025:11025:bandit level 25:/home/bandit25:/bin/bash
```

So `bandit26` will not executed `bash` when logged in. Instead, `/usr/bin/showtext` will be executed. What is `showtext` ? Let's see what it is.
```bash
cat /usr/bin/showtext
```

`/usr/bin/showtext` is a shell script :
```bash
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
```

Notice, `exit 0` which mean this shell script will be automatically turned off when hit `exit 0`.  We have to stop before `exit 0` so that we can turn on `bash`

- `more` is command that give you screenful view. If the data is not big enough to fit whole screen, this command doesn't have effect.  Since ~/text.txt is very short message, if you have big screen, `more` command is automatically turned off and execute `exit 0` --> Therefore make your screen very small using mouse or whatever. 

> 2. Now log in using private key and keep the screen very small size.

- Now, following step wasn't came up with by me and this is just solution.
	-When `more` is executed and user is in the interactive mode, pressing `v` will take user to the `vi-mode`

- In the `vi-mode`, you can check which shell is being used and also you can set different shell.
	- `set shell?` in the `vi` , will display which shell is being used.
	- `:set shell=/bin/bash` will set default program as `bash` when logged in to the server.
	- `:shell` will execute shell in `vi-mode`

>3. Now, `bandit26` 's bash is running .
>4. You can look at `/etc/bandit_pass/bandit26` : 26's password
>	Also, in a `home` directory, there is a `su` 27 that is similar to what we had seen in [[bandit#bandit Level19 --> Level20]] . Simply, by running command preceeding with `27do` in home directory, you get 27's authorization.

bandit26, `c7GvcKlw9mC7aUQaPx7nwFstuAIBw1o1` done.

bandit27,  `YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS` done.
