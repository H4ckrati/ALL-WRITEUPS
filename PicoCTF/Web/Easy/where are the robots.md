#Disallow
#### Challenge Description

Can you find the robots?
http://fickle-tempest.picoctf.net:65233

### First Step 

We know that the robots are in the page followd by /robots.txt so I did that and I found that : 

![](../../PicoCTF-assets/Pasted%20image%2020260222112604.png)

For your information : What is disallow in this case ?

In this specific context, **Disallow** is a command used in a **robots.txt** file to tell search engine crawlers (like Google or Bing) **not to visit or index** a specific part of a website. (It can be for many reasons as privacy)

#### Second Step 

So I decided to go in http://fickle-tempest.picoctf.net:65233/cc6b1.html to find the flag because its the page that is hidden


Flag : picoCTF{ca1cu1at1ng_Mach1n3s_cc6b1}
