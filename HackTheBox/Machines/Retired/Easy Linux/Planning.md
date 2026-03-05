#steps #step #crontab #cronjob #sshportforwarding #sshforward #portforwarder
#recap
#### Challenge Description 

[HTB Planning](https://app.hackthebox.com/machines/Planning) is an **easy** Linux machine that highlights an **RCE** on Grafana, a **container escape**, and privilege escalation via a **cronjobs** service to obtain system privileges.

We are provided with the following credentials : `admin:0D5oT70Fq13EvB5r`

## Enumeration

After an initial scan of all TCP ports on the box, I perform a service scan on the open ports to obtain more information about them:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*bZjh52RLLpprokRdzdr9BQ.png)

Service scan

After visiting the site, I added the name `planning.htb` to my hosts file and ran a vhost scan, which is often useful on HTB boxes:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*DQiL4YDBQN1ZeMBC3-h4jw.png)

vhost scan

This scan proved conclusive, as it allowed me to discover an available Grafana service and connect with the provided credentials:

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*kAuNR5LShOelp2vNiR6ipA.png)

Grafana login page

## CVE-2024–9264 on Grafana v11.0.0

Once logged in, it’s good practice to check whether the service version is vulnerable to a CVE, which is what I did right away. Bingo, Grafana v11.0.0 is vulnerable to CVE-2024–9264 !

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:760/1*A9TX0BjoY0Os6IFlcNrAoA.png)

Grafana version

This CVE is an authenticated RCE, and the following GitHub repository contains a very easy-to-use PoC : [https://github.com/nollium/CVE-2024-9264](https://github.com/nollium/CVE-2024-9264)

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*1B4dVw1v6HtwDjyvIt1Slg.png)

CVE-2024–9264 exploitation

The fact that the user is root already tipped me off that the service is running in a Docker environment.

## Get ruruuu’s stories in your inbox

Join Medium for free to get updates from this writer.

Subscribe

After several unsuccessful attempts at getting a reverse shell, and after verifying that the container contained the Perl binary, I generated a Perl script with [Claude](https://claude.ai/) :

![](https://miro.medium.com/v2/resize:fit:1394/1*IDJ4Sln8FA87VyRRNQi1zw.png)

Perl reverse shell

I then uploaded the shell to the machine via Python’s http.server module and found RCE :

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*yfi4JmJHEuXFDfLgL7dZvg.png)

Shell uploading

Then I executed it…

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*8wwHB36VZH6JDdyFlVtjag.png)

Getting a reverse shell

And finally, I got that long-awaited shell :

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*iNTy_zBscTehv915KacMbg.png)

Finally

## User flag

### Container evasion

Once on the container, I ran linpeas.sh to search for juicy information. Linpeas discovered credentials in environment variables, which are as follows : `enzo:RioTecRANDEntANT!`

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*bmexbHcexJ5c4WB_6e2SBg.png)

Linpeas results

With a bit of luck, I thought that Enzo would reuse his credentials on the SSH service, which was the case. This brings us to the user flag :

![](https://miro.medium.com/v2/resize:fit:886/1*bqNa0IkHWTg7Z-mmpiN4_A.png)

User flag

## Root flag

### Crontab UI

After exploring the file system a little, I found the `crontab.db` file in the `/opt/crontabs` directory :

![](https://miro.medium.com/v2/resize:fit:956/1*tMtSa8TgXoEy058BFJiBAA.png)

Crontab.db file

I obviously transferred this file to my machine and found some interesting information in it, such as `root_grafana`credentials for another service :

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*w3pTEBx88yfuLVjGDdipbQ.png)

Credentials found

Looking at the open ports on the server, I found port 8000, which I redirected to my machine using SSH port forwarding `ssh -L 8000:127.0.0.1:8000 enzo@planning.htb` :

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*Enx8y_EDlWxtZh6XhQ4uhw.png)

Cronjobs service

Since I know that these jobs run as root, I had the idea of creating a job that would copy the bash binary and add a SUID bit to it so that it could be executed with root privileges :

![](https://miro.medium.com/v2/resize:fit:1340/1*pZxWYQl_EJ_zG7nsT25gzQ.png)

Malicious job

After running the job, I can see that the binary is there :

![](https://miro.medium.com/v2/resize:fit:1350/1*ImwhXoEG7EFYWn3h7N72JA.png)

Malicious binary

And I finally get the root flag :

![](https://miro.medium.com/v2/resize:fit:1046/1*H3xFw788xCT5ADsETZMo3Q.png)

Root flag

#### RECAP :  ( of steps for a hacker to get root)

#### 1 step : 

```
sudo -l
```
#### 2 step :

```
ss -tl
ss -tlpn
```

