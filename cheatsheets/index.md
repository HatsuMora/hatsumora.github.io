---
layout: page
title: Cheatsheets
permalink: /cheatshees/
---

Nmap Scan ```sudo nmap -A -sS -p- IP -v```


Monitor who is pingin you sudo tcpdump -i tun0 icmp

Check sudo -l + https://gtfobins.github.io/


[Better shells](https://tryhackme.com/room/introtoshells): 
python -c 'import pty;pty.spawn("/bin/bash")'
export TERM=xterm
Ctrl + Z
stty raw -echo; fg