---
layout: post
title:  "[THM] Pickle Rick"
date:   2021-02-05 11:47:08 +0200
categories: THM
---

The [Pickle Rick room](https://tryhackme.com/room/picklerick) in TryHackMe is the second room you will do on your own when following the *complete beginner*  path.

Our task is to answer 3 questions: 

* What is the first ingredient Rick needs?
* Whats the second ingredient Rick needs?
* Whats the final ingredient Rick needs?

Remember to boot up your machine, and let's start.

Gathering Information
---------------------

1. Let's run ```sudo nmap -A -sS -p- IP -v``` to get an overview of the services running on the machine.

We can observe that there is 2 TCP port open on the machine, port ```22``` and ```80```, and the service scanner detects SSH and HTTP protocol running on those ports.

![Nmap scan](/assets/images/posts/pickle-rick/nmap.jpg)

Important to notice the version of the services ```Open SSH 7.2p2``` and ```Apache httpd 2.4.18```, we will look into them later, but now let's open a browser to inspect what information the website could offer us. 

![Pickle Rick welcome page](/assets/images/posts/pickle-rick/welcome.jpg)

Let's go ahead and open a *BURRRRRP* suite and see what can we do with it. After setting the browser to use burp proxy, we can go ahead and navigate the website, but as there is no links or any other information let's inspect the source code, we can do this on *Proxy > HTTP History* and then click on the Response tab, there is a **username** hiding there... Interesting.

We need to find more directories, for this task, you would like to know what technology is behind, the webserver, in this case, I am using a free tool called Wappalyzer to quickly check that.

![Wappalyzer](/assets/images/posts/pickle-rick/wappalyzer.jpg)

Now that I know that I have a hint that the backend is using PHP I will try to list available directories ```gobuster dir -x php,txt --url 10.10.224.220  --wordlist /usr/share/wordlists/dirb/common.txt```, I also added the extension .php to find PHP files (as seems like this website is using PHP), the .txt will find text files (usually look for files like robots.txt that hint crawler where not to go), and last but not least .html. After gobuster ran I can see ```http://IP/login.php``` 

![Wappalyzer](/assets/images/posts/pickle-rick/gobuster.jpg)

Some of the directories and files listed here we already knew from our *Taget > Site Map* on Burp suite, but gobuster also revealed some new findings like login.php, portal.php, robots.txt. After opening the new findings we can see that robots.txt has a string with no description, portal.php redirects to login.php, and login.php ask for username and password.

After trying the username from the index page and string found into robots.txt the login succeded, and now it is presented a panel that allows command execution on the backend. Let's have fun.

Getting the 3 magic ingridients
-------------------------------

First Ingredient
================

After trying a few commands on the web is worth to note something, some commands works, some others no (for example cat), but let's go for the first ingdridient, after running ```whoami```, it returns the user the webserver is running as, **www-data**, lets run ```ls -al``` to list the files in the current directory, eureka, we see a file with the first ingridient, to retreive it you will need a command that is not *cat*.

Second Ingredient
=================
It is worth notice that there is also a file called hint.txt, and it says that we must look around in the file system. We can see what users are available in the system by listing the /home directory, and we can see 2 different users, rick and ubuntu. Inside the ```/home/rick/second ingridient```  file you will find the second flag required for the room.

Third Ingredient
================
After roaming around on the file structure you will not find the third flag, then let's see what commands we can run as root, for this use the command ```sudo -l```, we will find that we can run any command as sudo, now let's inspect the root directory ```sudo ls -al /root``` and we will find a file called 3rd.txt. we can retreive the contents with ```sudo strings /root/3rd.txt```

![sudo -l](/assets/images/posts/pickle-rick/sudo.jpg)

And that's it, see you on the next room.

Answers:
--------
1. What is the first ingredient Rick needs? mr. meeseek hair
2. Whats the second ingredient Rick needs? 1 jerry tear
3. Whats the final ingredient Rick needs? fleeb juice