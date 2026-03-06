#Cluster #IPspoofing
#### Challenge Description 

Dear colleagues,

We’re now managing connections to the intranet using private IP addresses, so it’s no longer necessary to login with a username / password when you are already connected to the internal company network.

Regards,

The network admin

#### 1 Related Ressource :
https://repository.root-me.org/RFC/EN%20-%20rfc1918.txt


#### Research

According to the challenge description, I needed to find the good private IP to get the password.

I found a website which summarized what we needed to know : 

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260225162520.png)

from https://www.tencentcloud.com/techpedia/113791


#### Verification

I needed to see how to change the IP in the HTTP Request, so I then made some researched in google and I found that I can change my IP in BURP REPEATER  with this HTTP Header :

```
X-Forwarded-For: 212.212.212.2
```

Knowing that, let's see if the website detect that I changed the IP

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260225163044.png)

#### Execution 

I have a lot of IP to test because as we seen there is 3 options of IP :

So After some research I found that in BURP INTRUDER, I can test all combinaisons for example an IP 192.168.X.X  It will then test all combined combinaisons of the given IP with this : 

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260225164340.png)

I can chose Cluster bomb attack with this option which I can change by clicking on sniper attack on the top of BURP INTRUDER.

So let's try this with the given IP of 192.168.X.X 

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260225165235.png)

Now let's put the payload for the first && : 

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260225165308.png)


However I need to put the same payload for the second && : 

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260225165419.png)

Then run the attack

###### Initially : 

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260225165823.png)

##### Finally 

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260225165805.png)


You will found out that the length changed (570 to 565 )
The Length column shows the size of the HTTP response body (and sometimes headers, depending on the tool) in bytes.

#### Password

You then put in repeater the request that had length : 565 and send it you will se that the password is right there.

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260225165510.png)

Password : Ip_$po0Fing