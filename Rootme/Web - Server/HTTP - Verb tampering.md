#Methods #VerbTampering #Verb-Tampering #PUT 
#### Challenge Description :

Bypass the security establishment.


First, let's define Verb Tampering :

**HTTP verb tampering** is a security exploit where an attacker bypasses access controls by substituting standard request methods (like `GET` or `POST`) with less common ones that a server may not be configured to restrict. For example, if a developer secures a page against `POST` requests but forgets to block the `HEAD` method, an attacker could use a `HEAD` request to bypass the login prompt and view restricted content.

So let's try other HTTP Methods than GET :

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260306141700.png)

The PUT Method rendered me the flag ! Let's go !

Flag :  

```
a23e$dme96d3saez$$prap
```