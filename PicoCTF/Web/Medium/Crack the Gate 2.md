
#### Challenge Description

The login system has been upgraded with a basic rate-limiting mechanism that locks out repeated failed attempts from the same source. We’ve received a tip that the system might still trust user-controlled headers. Your objective is to bypass the rate-limiting restriction and log in using the known email address: **ctf-player@picoctf.org** and uncover the hidden secret.The website is running [here](http://amiable-citadel.picoctf.net:57516/). Can you try to log in?.Download the passwords list [here](https://challenge-files.picoctf.net/c_amiable_citadel/1501f7130808b3e2051d66dff10679f89414667b57f3418d4d6e14fbd691b822/passwords.txt).

#### First Step

So with the context of the Challenge Description I understood that I needed to use BURP INTRUDER to change my IP for each request using this HTTP Header : 

```
X-Forwarded-For: 212.212.212.2
```

And in the same time, I would need to change in the same time the password thanks to the file password.txt that we downloaded. 

##### BUT HOW can I change two values at the same time ? 


#### Second Step

![](../../PicoCTF-assets/Pasted%20image%2020260226000545.png)



![](../../PicoCTF-assets/Pasted%20image%2020260226000207.png)



flag : picoCTF{xff_byp4ss_brut3_3477bf15}