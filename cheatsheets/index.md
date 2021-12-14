---
layout: page
title: Cheatsheets
permalink: /cheatshees/
---

Nmap Scan ```sudo nmap -A -sS -p- IP -v```


Monitor who is pingin you sudo tcpdump -i tun0 icmp

Check sudo -l + https://gtfobins.github.io/

Find SUID:

find / -type f -perm -u=s 2>/dev/null

Learn more on how to get [Better shells](https://tryhackme.com/room/introtoshells) on this TryHackMe room.


* python3 -c 'import pty;pty.spawn("/bin/bash")'
* export TERM=xterm
* Ctrl + Z
* stty raw -echo; fg
