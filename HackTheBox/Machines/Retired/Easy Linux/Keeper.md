

```
cat Keeper.nmap 
# Nmap 7.98 scan initiated Wed Mar 11 04:17:21 2026 as: /usr/lib/nmap/nmap --privileged -sC -sV -oA nmap/Keeper 10.129.3.14
Nmap scan report for keeper.htb (10.129.3.14)
Host is up (0.024s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 35:39:d4:39:40:4b:1f:61:86:dd:7c:37:bb:4b:98:9e (ECDSA)
|_  256 1a:e9:72:be:8b:b1:05:d5:ef:fe:dd:80:d8:ef:c0:66 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```



![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311044113.png)




The default credentials for
Request Tracker (RT) version 4.4.4+dfsg-2ubuntu1 (often found in Ubuntu Jammy/22.04 environments) are: 

    Username: root
    Password: password 



![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311045610.png)

So let's try to login with SSH using her credentials :

lnorgaard / Welcome2023!



![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311045517.png)