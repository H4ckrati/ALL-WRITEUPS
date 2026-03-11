#scp #SCP #kpcli #puTTY #keepass #putty #PUTTY 
## Introduction

Before starting to present the way it has been taken to solve Keeper machine, I will present an outline with the concepts that will be used to solve challenge:

- Use of default credentials in the Request Tracker software.
- Connection to Keeper via SSH, due to a information leakage in Request Tracker.
- Keepass 2.x vulnerability exploit (CVE-2023-32784).
- Obtaining the root's Openssh private key from the PuTTy key stored in Keepass.

## Reconnaissance phase

As usual, reconnaissance phase was the first phase executed due to the fact that a hacker needs to know as much information about his target as possible before starting to exploit posible vulnerabilities.

To do this, you need to add a new line in the ''/etc/hosts'' file, where you define which IP is linked to which domain name. As shown in the following image, if you want to access keeper.htb, you are going to send a request to 10.10.11.227 really:

[![hosts.png](https://camo.githubusercontent.com/a7d08066643ff9d20ffd6103f999d461121ee4ae2c237d2ac56071c072e017ef/68747470733a2f2f692e706f7374696d672e63632f387a4268644d59742f686f7374732e706e67)](https://postimg.cc/SnR2k2kM)

As you can see, there is one more subdomain linked to the same IP. Why this is the case is explained below.

Next, Nmap is used to scan all ports on the server. The tool output is as follows:

[![nmap.png](https://camo.githubusercontent.com/c0f5a1fe83dd3b3c819151a18ff2676475f0f8aabd4b7474809471ccfcab59c0/68747470733a2f2f692e706f7374696d672e63632f4d475771516863332f6e6d61702e706e67)](https://postimg.cc/nXwy8SGq)

As shown in the last image, two open ports were discovered exposing two different services:

- Port 22: SSH service.
- Port 80: HTTP service.

Due to the fairly up-to-date version of SSH, it seems that the way to go is to perform a technology scan on port 80.

[![whatweb.png](https://camo.githubusercontent.com/6c67c3df7ae217cae2eeb4a75f0749f1a9bb2667591bbc0a553e0c1021c490da/68747470733a2f2f692e706f7374696d672e63632f54337952785a795a2f776861747765622e706e67)](https://postimg.cc/gxW9qtHD)

The tools do not yield much information, so the website was accessed to show what it contains:

[![Web.png](https://camo.githubusercontent.com/8d0d64b90ffad0d6df08e559884440bdffbd1762068d0d72fd74551f20543662/68747470733a2f2f692e706f7374696d672e63632f4e306d44336a514c2f5765622e706e67)](https://postimg.cc/Lq4jfRYM)

As you can see in the image, the subdomain to audit is tickets.keeper.htb, and so in /etc/hosts there are two domains linked to one IP.

By repeating the technological scan on port 80, but changing the subdomain, the following information can be seen:

[![whatweb2.png](https://camo.githubusercontent.com/202afd413c340c494ab65a7b78d205289f164f31e01065e926f641a8adfa15e0/68747470733a2f2f692e706f7374696d672e63632f434d48566b626e722f77686174776562322e706e67)](https://postimg.cc/3WR6H4B2)

The presence of request tracker software is notable. Therefore, the Internet was searched for vulnerabilities, but nothing was found. Due to the lack of information on vulnerabilities, another path was taken. Default root credentials were found:

- USER: root
- PASSWORD: password

Entering the credentials in the login panel found at [http://tickets.keeper.htb](http://tickets.keeper.htb/) would not give root access to the request tracking software. But, if you do a resource scan you may see new paths where it is possible to find new login panels, as it is shown below:

```shell
wfuzz -c -t 200 -hc 302 -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt http://tickets.keeper.htb/FUZZ
```

[![wfuzz.png](https://camo.githubusercontent.com/442ae8014e2e633584abf52e95d9a26713808775d5c5996447567a88b82d6aaf/68747470733a2f2f692e706f7374696d672e63632f426e7154543472442f7766757a7a2e706e67)](https://postimg.cc/crbtZy44)

## Exploitation Phase


At [http://tickets.keeper.htb/rt](http://tickets.keeper.htb/rt) there is another authentication panel that can be used to enter the root credentials and enter "request tracker" as this user. Once there, you can access as much information as possible, as the access was made with the user with maximum privileges, so all the ticket history was accessed and this information can be seen there:

[![Info.png](https://camo.githubusercontent.com/53cd203b56fae9ae3144ab407a383aea48d4f0847add570d9ff60298b87a4c01/68747470733a2f2f692e706f7374696d672e63632f5871514334357a4e2f496e666f2e706e67)](https://postimg.cc/q66RGNrW)

There seems to be a problem with a memory dump of the keepass program in a file located in Lise's home directory, so probably the way to hack this machine is to access lnorgaard's home and try to get the master password to decrypt the file and see the content. But how to access Lise's account? After logging into the website it is possible to go to the top bar and in the middle of it hover the mouse over the administration dropdown and then a subsection called Users appears. By clicking on it a new page appears with all the users registered in "Request Tracker". As you can see, the user lnorgaard appears, so click on him and inspect the user information. In a text box below you will see some credentials:

[![Leak.png](https://camo.githubusercontent.com/d91eaa79e6e7b01949e6beb4a906319df46bbda9e11cd594ad9a58ecbadcf889/68747470733a2f2f692e706f7374696d672e63632f504a5956333831322f4c65616b2e706e67)](https://postimg.cc/XBNgXqHC)

If you try to see if the credentials are reused in other services such as SSH, you will be able to log in with the user lnorgaard.

[![Access.png](https://camo.githubusercontent.com/fa88de19727aae5c4e45f343651057f42a0c0bf7857acce598b5b1f4d12ae463/68747470733a2f2f692e706f7374696d672e63632f394d733276544b392f4163636573732e706e67)](https://postimg.cc/2bdgWqzj)

## Post-Exploitation Phase

After gaining access to the user lnorgaard, it would be time to do a system recognition, but remember that there is a file in the Lise home path that contains the Keepass memory dump. So it's time to introduce CVE-2023-32784.

### CVE-2023-32784

The vulnerability affects all Keepass software versions below 2.54. The only requirement is to have a memory dump. It does not matter where the dump comes from, whether it is a process dump, a swap file (a dedicated space on a computer's storage device that is used as virtual memory. Virtual memory is a memory management technique that allows the operating system to use part of the storage device as if it were additional RAM), a hibernation file (this is a system file used to support the hibernation function. Hibernation is a power-saving mode in which the contents of a computer's RAM are saved to the hard disk or other non-volatile storage device before the system is shut down) or a RAM dump.

Keepass works with custom text boxes for many purposes, for example to enter the master password or to edit passwords, among others. When a person writes in these boxes, a leftover string is created in memory, which is practically impossible to delete. If you type "Password" seven leftover strings are created in memory:

- ·a
- ··s
- ···s
- ····w
- ·····o
- ······r
- ·······d

Guessing the first character will retrieve the master password in plain text.

To recover the leftover strings there is a script programmed in C# that will help you to automate this process ([https://github.com/vdohney/keepass-password-dumper](https://github.com/vdohney/keepass-password-dumper)).

```shell
dotnet run KeePassDumpFull.dmp --project <route_to_dotnet_project> #Running the above script
```

The following file with possible passwords was retrieved.

[![contrase-as.png](https://camo.githubusercontent.com/b5bfa67e9ba124a13250fc81518e008a856752d45fd7330227bf5ae26aaafb28/68747470733a2f2f692e706f7374696d672e63632f66545268436d43422f636f6e74726173652d61732e706e67)](https://postimg.cc/hh6wPJHd)

By searching the internet it is possible to find a typical Danish dish that contains the letters in the first line of the picture above. It is called Rødgrød med fløde, so if you proceed to enter this as a password in keepass you will not have access to it. But if you enter rødgrød med fløde you will see the entire contents of keepass decrypted.

[![Keepass.png](https://camo.githubusercontent.com/9e6462821f0d13774c76842262c869151ddfeedc7d70dae0292e35e6ffee50dc/68747470733a2f2f692e706f7374696d672e63632f73584842545371532f4b6565706173732e706e67)](https://postimg.cc/8JWP5F1P)

As you can see, there is a PuTTy key to connect to root user via SSH. Now, it is the turn to transform this key into an openssh key to connect to root's account.

```shell
puttygen clave.ppk -O public -o clave_id_rsa.pub # Generates openssh public key from puTTy key
puttygen clave.ppk -O private-openssh -o id_rsa # Generates openssh private key from puTTy key
```

Once both keys have been obtained, all that remains is to connect via SSH with the private key to root's account.

[![root.png](https://camo.githubusercontent.com/465c8a9d6fb6d5d537f740c572479c5a64b13d320da1ddfb3fa47ff172b3b1f4/68747470733a2f2f692e706f7374696d672e63632f744a3343367833462f726f6f742e706e67)](https://postimg.cc/wRBp83bB)
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