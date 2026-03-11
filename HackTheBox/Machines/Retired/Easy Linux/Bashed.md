

```
nmap 10.129.3.6                  
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-11 02:37 -0400
Nmap scan report for 10.129.3.6
Host is up (0.023s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 1.16 seconds

```



```
gobuster dir -u http://10.129.3.6/ --wordlist /usr/share/seclists/Discovery/Web-Content/common.txt       
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.3.6/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8.2
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
.htaccess            (Status: 403) [Size: 294]
.htpasswd            (Status: 403) [Size: 294]
.hta                 (Status: 403) [Size: 289]
css                  (Status: 301) [Size: 306] [--> http://10.129.3.6/css/]
dev                  (Status: 301) [Size: 306] [-->http://10.129.3.6/dev/](Status: 403) [Size: 298]
uploads              (Status: 301) [Size: 310] [--> http://10.129.3.6/uploads/]
Progress: 4750 / 4750 (100.00%)
===============================================================
Finished
===========
```




![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311025104.png)







![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311024849.png)


